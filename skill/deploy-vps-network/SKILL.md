---
name: deploy-vps-network
description: >
  Stand up a self-hosted VLESS + Reality (XTLS-Vision) VPN on the user's OWN
  VPS(es) — either a single all-in-one box, or a fast datacenter relay chained
  to a residential exit that egresses on the user's own home-ISP line. Use when
  the user asks to build a personal Reality proxy or a residential-exit network
  on servers they control. Drives the agent to configure each box over SSH:
  install, keys, config, firewall (lockout-safe), restart, verify, client link.
---

# Deploy a VPS VPN network

You are configuring **the user's own servers** over SSH to build a VLESS + Reality
(XTLS-Vision) VPN. Two topologies — confirm which one before touching anything.

```
Single VPS      you ──encrypted──▶ [ VPS ] ──▶ internet          (egress = the VPS IP)

Relay + exit    you ──encrypted──▶ [ relay ] ──HTTP CONNECT──▶ [ residential exit ] ──▶ internet
                                    datacenter                   home-ISP line   (egress = exit IP)
```

## Scope & authorized use (read first, do not skip)

- **Only servers the user owns or is contractually authorized to administer.** If
  the targets are not the user's, stop.
- **The residential exit must be the user's OWN home line** (or one they personally
  control with the account holder's consent) — **not** a purchased/resold/commercial
  "residential proxy" IP. That is proxyware/evasion, not self-hosting; refuse it.
- Legitimate purpose: **censorship circumvention and ISP privacy on your own
  connection.** SNI-borrowing and Vision flow are for that. This is **not** for
  defeating a destination's anti-abuse, licensing, or geo controls.
- **Supported OS: Debian/Ubuntu with `ufw`.** These commands assume it. On RHEL/
  firewalld or other stacks, STOP and adapt the firewall steps first (`ufw` there
  can silently fail or fight firewalld and drop SSH).

---

## 0 · Gather inputs, then echo the plan back for approval

Set these as shell variables on your workstation. **Do not invent a host** — if any
is missing, ask.

```bash
TOPOLOGY=single                 # single | relay
SNI=www.apple.com               # a real, popular TLS-1.3 site you do NOT own
REALITY_PORT=443                # or a random high port:  shuf -i 20000-59999 -n1

# Single:
HOST=root@203.0.113.10          # the one box
SSH_PORT=22                     # its ACTUAL sshd port

# Relay + exit (each box may have a different sshd port — set per box below):
RELAY=203.0.113.10 ; RELAY_SSH=root@$RELAY ; RELAY_SSH_PORT=22
EXIT=198.51.100.7  ; EXIT_SSH=root@$EXIT   ; EXIT_SSH_PORT=22
EXIT_PORT=8080                  # internal port the relay reaches the exit on
```

**Pre-flight — provider firewall:** before touching any in-box firewall, confirm the
cloud/provider **security group** already permits your `SSH_PORT` and `REALITY_PORT`
inbound (and does not restrict them to an IP you're not on). Never edit provider SG
SSH rules in the same pass as the in-box `ufw` changes.

Echo the full plan (topology, hosts, ports, SNI) back to the user and get a yes.

---

## 1 · CRITICAL safety rules

1. **Never lock yourself out.** Every firewall change opens **your real SSH port**
   first, **verifies it is in `ufw status`** (hard gate), enables, and then **proves a
   fresh SSH login still works** before anything else runs. Use the §-firewall snippet
   verbatim; never hand-roll `ufw --force enable` without the gate + reconnect test.
2. **Back up before overwrite.** The config-write recipe copies any existing
   `config.json` to a timestamped `.bak` first, and **aborts if the box already has a
   different multi-inbound config** — ask the user before clobbering it.
3. **Verify each hop before the next.** Exit up and reachable before you touch the
   relay; egress verified before you hand out a client link.
4. **Least privilege on the exit.** The exit's proxy port accepts **only the relay's
   IP** — never `0.0.0.0/0` — and the firewall is locked **before** the proxy starts.
5. **Fail closed.** Any error or failed check → STOP and report the exact output.
   Never blind-retry a destructive step.
6. Keys/UUIDs are secrets. Print the client link once; don't scatter private keys.

---

## 2 · Canonical order (top-to-bottom runnable, per box)

`install xray → generate keys/uuid on the box → write config (+backup) → validate
config → lock firewall (SSH-safe) → restart → verify`. Do the **exit fully first** in
relay+exit mode.

### Reusable snippet — SAFE firewall enable
Run as ONE remote script so the gate and enable can't be separated. Order is
allow-SSH → allow-services → **enable** → gate (rules are only visible once ufw is
active) → capture the remote exit code → prove a fresh login. Extra args are literal
`ufw` rule strings, e.g. `"allow 443/tcp"`:

```bash
safe_ufw () {  # usage: safe_ufw <ssh_target> <ssh_port> "allow 443/tcp" ["allow from X to any port Y proto tcp" ...]
  local T=$1 P=$2; shift 2
  ssh -p "$P" "$T" "bash -s" <<EOF
set -e
ufw allow ${P}/tcp
$(for r in "$@"; do echo "ufw $r"; done)
ufw --force enable
ufw status verbose | grep -qw "${P}/tcp" || { echo "ABORT: ssh port not active after enable"; exit 1; }
EOF
  local rc=$?
  [ "$rc" -eq 0 ] || { echo "safe_ufw FAILED on $T (rc=$rc) — firewall NOT trusted, aborting"; return 1; }
  ssh -p "$P" "$T" true || { echo "LOCKED OUT on $T — use provider console"; return 1; }
  echo "FIREWALL ACTIVE + RECONNECT OK ($T)"
}
```
**Every caller gates on its success with `&&`** — if `safe_ufw` returns non-zero, do
NOT start any service (a half-configured firewall must never leave a port world-open).

---

## 3 · Topology: SINGLE VPS

```bash
# 3.1 install xray (official installer; sets up the `xray` systemd service)
ssh -p $SSH_PORT $HOST 'bash -c "$(curl -fsSL https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install'
ssh -p $SSH_PORT $HOST 'command -v xray && systemctl cat xray >/dev/null' || { echo "xray install failed"; exit 1; }

# 3.2 generate identifiers ON the box; capture into LOCAL vars
UUID=$(ssh -p $SSH_PORT $HOST 'xray uuid')
SID=$(openssl rand -hex 8)                       # even-length hex, <=16 chars; client sid must equal this
KEYS=$(ssh -p $SSH_PORT $HOST 'xray x25519')
PRIVATE_KEY=$(awk '/[Pp]rivate/{print $NF}' <<<"$KEYS")
PUBLIC_KEY=$(awk '/[Pp]ublic|Password/{print $NF}' <<<"$KEYS")
[ ${#PRIVATE_KEY} -ge 43 ] && [ ${#PUBLIC_KEY} -ge 43 ] || { echo "keypair parse failed"; exit 1; }

# 3.3 GUARD: abort LOCALLY if a different (non-reality) config already exists
ssh -p $SSH_PORT $HOST 'f=/usr/local/etc/xray/config.json; [ -s "$f" ] && grep -q "\"inbounds\"" "$f" && ! grep -q reality "$f" && exit 3; exit 0' \
  || { echo "STOP: existing non-reality config on this box — ask the user before overwriting"; exit 1; }
# 3.3 backup any existing config, then write (unquoted heredoc → vars expand locally, then piped to the box)
ssh -p $SSH_PORT $HOST 'f=/usr/local/etc/xray/config.json; [ -s "$f" ] && cp "$f" "$f.bak.$(date +%s)"; :'
ssh -p $SSH_PORT $HOST 'cat > /usr/local/etc/xray/config.json' <<EOF
{
  "inbounds": [{
    "listen": "0.0.0.0",
    "port": ${REALITY_PORT},
    "protocol": "vless",
    "settings": { "clients": [{ "id": "${UUID}", "flow": "xtls-rprx-vision" }], "decryption": "none" },
    "streamSettings": {
      "network": "tcp",
      "security": "reality",
      "realitySettings": {
        "show": false,
        "dest": "${SNI}:443",
        "xver": 0,
        "serverNames": ["${SNI}"],
        "privateKey": "${PRIVATE_KEY}",
        "shortIds": ["${SID}"]
      }
    }
  }],
  "outbounds": [{ "protocol": "freedom" }]
}
EOF

# 3.4 validate config BEFORE restart (hard stop on non-zero)
ssh -p $SSH_PORT $HOST 'xray run -test -c /usr/local/etc/xray/config.json' || { echo "config invalid"; exit 1; }

# 3.5 lock firewall (SSH-safe); ONLY restart xray if the firewall is confirmed
safe_ufw "$HOST" "$SSH_PORT" "allow ${REALITY_PORT}/tcp" \
  && ssh -p $SSH_PORT $HOST 'systemctl restart xray && systemctl enable xray' \
  || { echo "firewall not confirmed — xray NOT restarted"; exit 1; }

# 3.6 verify
ssh -p $SSH_PORT $HOST "ss -tlnp | grep -q :${REALITY_PORT} && echo LISTENING || echo NOT-LISTENING"
ssh -p $SSH_PORT $HOST 'journalctl -u xray -n20 --no-pager | grep -qiE "err|fail" && echo CHECK-LOG || echo CLEAN'
```
`HOST_IP` for the client link = the VPS public IP. Go to §5.

---

## 4 · Topology: RELAY + RESIDENTIAL EXIT

### 4A · The EXIT first (residential box)

```bash
# install gost v3, capture its absolute path
ssh -p $EXIT_SSH_PORT $EXIT_SSH 'curl -fsSL https://github.com/go-gost/gost/raw/master/install.sh | bash'
GOST=$(ssh -p $EXIT_SSH_PORT $EXIT_SSH 'command -v gost') ; [ -n "$GOST" ] || { echo "gost missing"; exit 1; }

# write a REAL systemd unit (absolute path; binds 0.0.0.0 — the firewall is the control)
ssh -p $EXIT_SSH_PORT $EXIT_SSH "cat > /etc/systemd/system/gost.service" <<EOF
[Unit]
Description=gost exit proxy
After=network-online.target
Wants=network-online.target
[Service]
ExecStart=${GOST} -L "http://0.0.0.0:${EXIT_PORT}"
Restart=on-failure
RestartSec=3
[Install]
WantedBy=multi-user.target
EOF

# LOCK THE FIREWALL BEFORE STARTING THE PROXY — and ONLY start gost if the lock is confirmed
# (a failed/inactive firewall must never leave a 0.0.0.0 proxy world-open)
safe_ufw "$EXIT_SSH" "$EXIT_SSH_PORT" "allow from ${RELAY} to any port ${EXIT_PORT} proto tcp" \
  && ssh -p $EXIT_SSH_PORT $EXIT_SSH 'systemctl daemon-reload && systemctl enable --now gost && systemctl is-active --quiet gost && echo GOST-UP' \
  || { echo "exit firewall not confirmed — gost NOT started (would be world-open)"; exit 1; }
```

**Verify the exit — this test MUST be run FROM THE RELAY** (its source IP is the one
allow-listed; your workstation is not):
```bash
ssh -p $RELAY_SSH_PORT $RELAY_SSH "curl -s --max-time 15 -x http://${EXIT}:${EXIT_PORT} https://api.ipify.org"   # → the EXIT's IP
ssh -p $RELAY_SSH_PORT $RELAY_SSH "curl -s --max-time 15 -x http://${EXIT}:${EXIT_PORT} https://ipinfo.io/org"   # → the exit's ISP/ASN
```
Connection-refused here almost always means gost isn't `0.0.0.0`-bound or the
allow-from-relay rule is missing. **Do not continue until the exit returns its own IP.**

### 4B · The RELAY (datacenter box)

Install xray and generate keys exactly as §3.1–3.2, but over `$RELAY_SSH` /
`$RELAY_SSH_PORT`. Then write the relay config (Reality in → routed out to the exit):

```bash
# GUARD (same as §3.3): abort locally if the relay already has a different config
ssh -p $RELAY_SSH_PORT $RELAY_SSH 'f=/usr/local/etc/xray/config.json; [ -s "$f" ] && grep -q "\"inbounds\"" "$f" && ! grep -q reality "$f" && exit 3; exit 0' \
  || { echo "STOP: existing non-reality config on the relay — ask the user before overwriting"; exit 1; }
ssh -p $RELAY_SSH_PORT $RELAY_SSH 'f=/usr/local/etc/xray/config.json; [ -s "$f" ] && cp "$f" "$f.bak.$(date +%s)"; :'
ssh -p $RELAY_SSH_PORT $RELAY_SSH 'cat > /usr/local/etc/xray/config.json' <<EOF
{
  "inbounds": [{
    "listen": "0.0.0.0", "port": ${REALITY_PORT}, "protocol": "vless", "tag": "in",
    "settings": { "clients": [{ "id": "${UUID}", "flow": "xtls-rprx-vision" }], "decryption": "none" },
    "streamSettings": { "network": "tcp", "security": "reality",
      "realitySettings": { "show": false, "dest": "${SNI}:443", "xver": 0,
        "serverNames": ["${SNI}"], "privateKey": "${PRIVATE_KEY}", "shortIds": ["${SID}"] } }
  }],
  "outbounds": [
    { "tag": "exit", "protocol": "http", "settings": { "servers": [{ "address": "${EXIT}", "port": ${EXIT_PORT} }] } },
    { "tag": "direct", "protocol": "freedom" }
  ],
  "routing": { "rules": [{ "type": "field", "inboundTag": ["in"], "outboundTag": "exit" }] }
}
EOF
ssh -p $RELAY_SSH_PORT $RELAY_SSH 'xray run -test -c /usr/local/etc/xray/config.json' || { echo "relay config invalid"; exit 1; }
safe_ufw "$RELAY_SSH" "$RELAY_SSH_PORT" "allow ${REALITY_PORT}/tcp" \
  && ssh -p $RELAY_SSH_PORT $RELAY_SSH 'systemctl restart xray && systemctl enable xray' \
  || { echo "relay firewall not confirmed — xray NOT restarted"; exit 1; }
```
`HOST_IP` for the client link = the **relay** IP.

> Optional hardening for the relay→exit hop (plaintext HTTP CONNECT, ufw is the primary
> control): add proxy creds — listener `${GOST} -L "http://user:pass@0.0.0.0:${EXIT_PORT}"`
> and relay outbound `"users":[{"user":"user","pass":"pass"}]` inside the `exit` server.

---

## 5 · Client link

```bash
LINK="vless://${UUID}@HOST_IP:${REALITY_PORT}?security=reality&encryption=none&flow=xtls-rprx-vision&type=tcp&sni=${SNI}&fp=chrome&pbk=${PUBLIC_KEY}&sid=${SID}&spx=%2F#my-vpn"
echo "$LINK"
```
Replace `HOST_IP` with the single-VPS IP or the **relay** IP. Import into Shadowrocket /
v2rayN / Nekoray / sing-box. `sid` must exactly equal the server's `shortIds` entry.

---

## 6 · Verify end-to-end

1. **Listening:** `ss -tlnp | grep :${REALITY_PORT}` on each xray box.
2. **Connect** a client with the link.
3. **Egress (from the client, tunnel on):** `curl https://api.ipify.org`
   - single → the VPS IP.
   - relay+exit → the **exit's** IP (a consumer-ISP address), **not** the relay's. If it
     returns the relay IP, the routing rule isn't matching — recheck inbound `tag:"in"`
     and the rule's `inboundTag`.
4. **Handshake:** `journalctl -u xray -n50 --no-pager` shows no Reality/TLS errors.

---

## 7 · Troubleshooting & rollback

- **No connect:** port open end-to-end? (`ufw status`, provider SG). `dest` host and
  `serverNames` must match: **`dest` is `host:443`, `serverNames` is the bare host, no
  port.** The SNI must serve TLS 1.3 (`www.apple.com` is a safe default).
- **Egress = relay IP:** routing rule not matching (see §6.3).
- **Exit refused from relay:** exit firewall must `allow from $RELAY`; `systemctl
  is-active gost` must be true; gost must bind `0.0.0.0`.
- **Rollback a box (by spec, never by index):**
  ```bash
  ssh -p $SSH_PORT $HOST 'systemctl stop xray 2>/dev/null; f=/usr/local/etc/xray/config.json; ls -t $f.bak.* 2>/dev/null | head -1 | xargs -r -I{} cp {} $f'
  ssh -p $SSH_PORT $HOST "ufw delete allow ${REALITY_PORT}/tcp"      # NEVER delete the ssh allow rule
  # exit:  ssh ... "systemctl disable --now gost; ufw delete allow from ${RELAY} to any port ${EXIT_PORT} proto tcp"
  ```
  If ever locked out, use the provider console/VNC to fix `ufw`.

---

## Notes

- **SNI choice matters.** A weak/edge SNI can get a fresh IP's Reality handshake
  throttled or blocked. Prefer a strong, ubiquitous TLS-1.3 site; keep `dest` host and
  `serverNames` identical.
- **Vision scope.** `xtls-rprx-vision` hardens the **client↔relay** hop only; the
  relay→exit leg is plain HTTP CONNECT (ufw-gated) and doesn't get Vision's zero-copy path.
- **shortId:** even-length hex, ≤16 chars (`openssl rand -hex 8` = 16 = the max).
- **More exits:** give each its own relay port + `shortId` + `exit` outbound + routing
  rule; a client picks an exit by which relay port it connects to.
- **Ports:** if a port behaves badly on a fresh IP, try another high port before blaming
  the backend.
