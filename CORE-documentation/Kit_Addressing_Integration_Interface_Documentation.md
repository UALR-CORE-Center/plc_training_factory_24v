# Kit Addressing & Integration Interface Contract (One-Kit MVP)

Oct 12, 2026 · @Caellum

## Scope and status

This contract fixes the four interfaces the one-kit MVP needs: hardware versions, network addressing, Agoge–Pi, and HMI–Pi. Contract version **0.2 (draft)**; it becomes 1.0 when the Agoge, Pi, and HMI implementers sign off in the last section.

How to read it:

- **Proposed** = a value chosen here for agreement. Change it before sign-off, not after.
- **Record** = a fact to capture from the physical reference kit. It is not a design choice.
- The Pi is the integration hub. Agoge talks only to the Pi (outbound HTTPS from the Pi). The HMI talks only to the Pi (OPC UA). Students reach equipment only through Agoge: they sign in, open their lab's Kali machine, and that machine reaches the kit through the WireGuard tunnel via the cloud hub wg\<n>, run by Agoge. The Pi enforces the lease's authorized routes; for the MVP that is the whole kit network (decided Oct 27, 2026).

Reference kit: the values recorded on Oct 16 and 19 come from the presentation rig, which has no WireGuard. A second rig, used for development, has WireGuard installed. Which rig is the reference kit is an open decision; the development rig's Pi was checked on Oct 20 and matches the presentation rig (model, OS, kernel, Node.js, Node-RED) apart from WireGuard. The development rig currently has no HMI fitted, so all HMI values come from the presentation rig's MTP700; the same panel model must be fitted to the development rig before HMI–Pi testing. Out of scope for the MVP: more than one kit, PLC program logic, and the student-facing Agoge UI.

## Reference kit versions

Every row below must be filled from the physical reference kit before sign-off; implementers build and test against exactly these versions.

| Component | Model / order number | Firmware or software version | How to read it |
| --- | --- | --- | --- |
| Kit | fischertechnik Training Factory Industry 4.0 24V, complete set with PLC S7-1500 | — | [Vendor repo](https://github.com/fischertechnik/plc_training_factory_24v) |
| PLC | SIMATIC ET 200SP, CPU 1512SP F-1 PN, `6ES7 512-1SM03-0AB0` (confirmed on the module), bus adapter BA 2×RJ45, 13 I/O modules (photo) | Firmware V4.1.2, hardware version 6 (confirmed online in TIA Portal); loaded project version V21.0.0.0 | TIA Portal → Online & diagnostics → General |
| PLC project | fischertechnik `LearningFactory_4_0_24V_v15_TP_V18.zap18`, with the HBW horizontal-axis fix in FB14 | TIA Portal V21 with STEP 7 Professional V21 (upgraded from V18), on engineering laptop ET-ISAC-05; program version index 1.5 | TIA Portal → Help → Installed software |
| HMI — Siemens MTP700 Unified Comfort | 6AV2128-3GB06-0AX1, S/N LBU4069131, base MAC 30-13-89-50-F0-A8 (rear label) | Image V20.00.00.00\_98.05.00.01, Runtime V20.0.0.0, bootloader V09.05.00.00\_01.01.01.01 (01/16/2026); read on the panel's Control Panel on Oct 22, 2026. Network connection uses port PN-X2 (MAC 30-13-89-50-F0-AB); PN-X1 is 30-13-89-50-F0-A8 | Panel settings → device info |
| HMI project | None seen — the panel runs Firefox showing the Pi's Node-RED dashboard | WinCC Unified V21 installed on ET-ISAC-05, ready if an HMI project is built. The panel runs a V20 image, so a project must target V20, or the panel needs a V21 image update first | TIA Portal → Help → Installed software |
| Router | TP-Link TL-WR902AC (AC750 travel router), `hardware v4, LAN MAC 7C:F1:7E:34:1A:5F, 2.4 GHz SSID TP-Link_1A5F, 5 GHz SSID TP-Link_1A5F_5G (hardware notes say TP-Link_8FDD, out of date)` | 0.9.1 0.3 v0089.0 Build 230506 Rel.55329n(4555); operation mode WISP | Router admin page → status |
| TXT 4.0 Controller | fischertechnik TXT 4.0, controller ID TXT4.0-Xq93 | Firmware 3.1.10 (API 6.4.2, Linux 4.19.49, bootloader 2018.11); runs Node-RED, MQTT broker, `Nfc_MQTT.py` | TXT touchscreen → settings / about |
| Raspberry Pi | Raspberry Pi 4 Model B Rev 1.5 on both rigs (hostname `IOTpi2` on the presentation rig, `labpi` on the development rig) | — | `cat /proc/device-tree/model` |
| Pi OS | Raspbian GNU/Linux 11 (bullseye), 32-bit (`armv7l`) | Kernel `5.10.63-v7l+`, built Nov 2021 | `cat /etc/os-release` and `uname -r` |
| Pi services | Node-RED (fischertechnik flows `flows_IOTpi2.json` + dashboard), `ntp` local time server | Node-RED v1.0.4 on Node.js v12.22.9; node-red-dashboard 2.19.4; fischertechnik dashboard version index 1.4 | Node-RED startup log |
| WireGuard | — | Not installed on the presentation rig. Development rig: wireguard-tools v1.0.20210223. As of Oct 30, 2026 a wg0 tunnel starts at boot (listen port 50603) with one peer, the Agoge test hub at 136.115.55.196:51820, tunnel address 10.254.254.2/32, allowed IPs 10.20.0.0/24, 25 s keepalive. No handshake yet (0 B received) because the Pi had no IPv4 route to the internet: its gateway 192.168.0.252 was unreachable. On Nov 3, 2026 the rig had internet, and SSH to pi@192.168.0.5 from the Agoge test lab's desktop (a Windows machine in the browser) succeeded through the tunnel: the end-to-end path works (wg show: latest handshake 9 s ago, 4.47 KiB received) | `wg --version` |
| Pi gateway agent (Agoge client + OPC UA server) | — | Not built yet; Record at release | `<agent> --version` |
| OPC UA server library on Pi | — | Not chosen yet; Record at release | Agent dependency lock file |
| Agoge API | — | `v1` (Proposed) | Response header `Agoge-Api-Version` |
| This contract | — | 0.2 | This doc |

A kit counts as a reference kit only if every row matches; any upgrade is a contract change.

## Network and addressing

Every kit keeps the fischertechnik equipment LAN `192.168.0.0/24` as recorded on the reference kit, so vendor flows and PLC/HMI projects need no re-addressing; only the WireGuard tunnel subnet and gateway name change per kit number `<n>` (MVP: `n = 1`).

| Item | Value | Status | Notes |
| --- | --- | --- | --- |
| Equipment LAN | `192.168.0.0/24` | Recorded | fischertechnik default. The home-network collision does not apply, because students connect from Agoge lab machines; see MVP defaults. |
| PLC (CPU 1512SP F-1 PN) | `192.168.0.1` | Recorded | Set via TIA Online & diagnostics → Assign IP. Also hosts the PLC's own OPC UA server. |
| Router (LAN gateway) | `192.168.0.252` | Recorded | TP-Link TL-WR902AC. Moved off `.1` so the PLC can use it. Runs in WISP mode: it joins an upstream Wi-Fi as a client and NATs the kit behind it (on Oct 16 the upstream link was not connected and the WAN address was 0.0.0.0, so the kit had no internet). |
| Raspberry Pi (stable local address) | `192.168.0.5` | Recorded | Static, outside the DHCP pool. On the development Pi, Node-RED listens on 127.0.0.1 only (Nov 3, 2026), so students cannot open the dashboard over the tunnel until that setting changes. Development rig uplink (Nov 3, 2026): the Pi reaches the internet through the building network on eth0 (second address 10.1.11.187/23, gateway 10.1.10.1), not through the kit router; a stale default route via 192.168.0.252 had to be removed by hand (steps in ISP.txt on the Pi) and may return after a reboot. The Agoge student lab machine connected from 10.20.0.2. Node-RED on 1880, dashboard `/ui/`. |
| TXT 4.0 Controller | `192.168.0.10` | Recorded | DHCP reservation on the router. MQTT broker on 1883. |
| HMI (MTP700 Unified) | `192.168.0.13` | Recorded | Found by ARP scan; network MAC `30-13-89-50-F0-AB` (label shows base MAC ending `A8`). Kept stable by a DHCP reservation on the router (added Oct 19, 2026). |
| DHCP pool | `192.168.0.10`–`.199` | Recorded | Changed from `.100`–`.199`. Laptops joining the kit Wi-Fi land here too. |
| Free static ranges | `.2`–`.4`, `.6`–`.9`, `.200`–`.251` | Derived | Use these for any new equipment. |
| WireGuard tunnel subnet | `10.88.<n>.0/24` | Proposed | Hub is `10.88.<n>.1`, Pi is `10.88.<n>.2`; leased student peers `.10`–`.254`, assigned by Agoge. The Agoge test hub actually gives the Pi 10.254.254.2/32 and routes student lab machines from 10.20.0.0/24. |
| Gateway DNS name | `wg<n>.<environment DNS suffix>` | Proposed | e.g. `wg1.<prod suffix>`. Resolves to the cloud WireGuard hub for kit n. |
| Environment DNS suffixes | Record | Open | One per environment (dev, staging, prod). |
| WireGuard listen port | UDP `51820` on the cloud hub | Proposed | Decided Oct 21, 2026. The Pi connects out to the hub with a 25 s keepalive, so the kit needs no inbound port forward and works behind the WISP router or a hotspot. The Agoge test hub was at 136.115.55.196:51820 on Oct 30 and at 35.253.5.173:51820 on Nov 3. Its public IP changes, so production kits should point at the wg\<n> DNS name or a static IP; the Pi's tunnel address is 10.254.254.2/32, and it accepts traffic from 10.20.0.0/24, likely the Agoge network where student Kali machines sit (seen on the development rig, Oct 30, 2026). |
| Agoge API origin | `https://api.<environment DNS suffix>` | Proposed | Pi calls out on TCP 443 only. Exact hostname: Record. |
| Time source | Pi `ntp` server (`192.168.0.5`) | Recorded | PLC NTP Server 1 and TXT both point at the Pi. Without it, TXT clock drift makes `Nfc_MQTT.py` reject NFC commands (60 s limit). |

Rules:

- The Pi forwards tunnel traffic to the equipment LAN only for routes a valid lease authorizes; everything else is dropped.
- The Pi NATs (masquerades) tunnel traffic onto the LAN, so PLC, TXT, and HMI need no route back to `10.88.<n>.0/24`.
- Equipment needs no internet once the Pi NTP fix is in place; the router may then block LAN egress except from the Pi.
- Credentials for kit devices live in the lab team's secret store, never in this contract.

### Existing data paths on the kit (Recorded)

The Pi already reads the PLC: the Node-RED flow `Digital Twin - MQTT Stream` polls the PLC OPC UA server every 100 ms and republishes to the MQTT broker on the TXT. The new Pi OPC UA server for the HMI must not disturb this flow.

| Path | Endpoint | Detail |
| --- | --- | --- |
| PLC OPC UA server | `opc.tcp://192.168.0.1:4840` | Namespace `ns=3`. Structured tags `"gtyp_<STATION>"."<Axis>"."<Variable>"`, flat I/O tags `"<PLC_Tag>"`. |
| Digital twin stream | Pi Node-RED → `192.168.0.10:1883` | Topics `fl/i/dt/<station>/pos` and `fl/i/dt/phs/act`, JSON payloads. |

Full tag and topic lists: [Digital Twin – OPC UA Address Space](https://github.com/UALR-CORE-Center/plc_training_factory_24v/blob/docs-marty/CORE-documentation/Digital%20Twin%20-%20OPC%20UA%20Address%20Space.md) and [Digital Twin – MQTT Topics](https://github.com/UALR-CORE-Center/plc_training_factory_24v/blob/docs-marty/CORE-documentation/Digital%20Twin%20-%20MQTT%20Topics.md).

### Troubleshooting: development rig Pi has no internet

Symptom: `sudo wg show` shows no “latest handshake” and 0 B received, or `ping 8.8.8.8` says Destination Host Unreachable. Cause seen on Nov 3, 2026: a stale default route via the kit router `192.168.0.252`, which has no uplink. The Pi's notes file `ISP.txt` holds the same steps.

1. Log in at the Pi's console as `pi` (password in the team's private notes, never in this doc).
2. Check routes with `ip route`. There should be one default route, via `10.1.10.1`.
3. If there is a default via `192.168.0.252`, remove it:

   ```
   sudo dhclient eth0
   sudo ip route del default via 192.168.0.252 dev eth0
   ```
4. Confirm with `ip route` and `ping -c3 8.8.8.8`.
5. Check the tunnel and dashboard: `sudo wg show` should show a recent “latest handshake”, and `sudo systemctl status nodered.service` should say active (running).

This fix is manual and may need repeating after a reboot until the route is made permanent.

## Agoge–Pi contract

Agoge test environment (recorded Oct 26, 2026): https://app.agoge-labs.com/test-dev. Its current flow: an admin registers the kit's gateway (Admin → Build Manager → Remote Labs); a teacher builds a "Wireguard Remote Lab" (Teachers → New Lab) with the gateway's WireGuard public key and the kit's local network CIDR (192.168.0.0/24); students join with a link and code, and their browser-based lab reaches the kit through the tunnel. Gateway registration fields: remote lab name and description, remote peer ID (e.g. remote-lab-01), remote device identity (serial number or other unique ID), remote LAN CIDR (Agoge rejects CIDRs that overlap an existing remote lab, so identical kit LANs only work one kit at a time), the gateway's WireGuard public key (the private key never leaves the Pi), and authorized instructor emails. One remote lab, remote-lab-01 on 192.168.50.0/24, was already registered on Oct 26. The development rig's Pi is registered as wireguard-test-01 (device identity 10000000cb72a587, the Pi's serial number; LAN 192.168.0.0/24) and is already assigned to a test lab (checked Nov 2, 2026). remote-lab-01 is a separate test entry and not part of this kit. The contract below must be reconciled with this flow; Agoge authorizes the whole CIDR, which matches the Oct 27 decision that students reach every device on the kit. The Pi pulls its configuration and leases from Agoge over authenticated HTTPS and acknowledges what it applied; Agoge never connects in to the Pi.

### Authentication

- TLS 1.2 or later to the Agoge API origin; the Pi verifies the server certificate against the system CA store.
- Each kit has a `kitId` and a device token issued at enrollment, sent as `Authorization: Bearer <token>`. The token is stored root-only on the Pi (`0600`) and rotatable from Agoge.
- Every request carries `Agoge-Kit-Id`, `Agoge-Agent-Version`, and a unique `X-Request-Id`.
- Proposed upgrade after MVP: replace bearer tokens with mTLS client certificates.

### Endpoints (base `https://api.<environment DNS suffix>/v1`)

| Method and path | Purpose | Pi behaviour |
| --- | --- | --- |
| `GET /kits/{kitId}/config` | Kit configuration | Poll every 60 s with `If-None-Match`; `304` = unchanged. |
| `GET /kits/{kitId}/leases` | All current and future leases | Poll every 15 s with `If-None-Match`. |
| `POST /kits/{kitId}/leases/{leaseId}/ack` | Report a lease applied, rejected, or removed | Send within 5 s of the change; retry with backoff. |
| `POST /kits/{kitId}/status` | Pi, PLC, HMI, and tunnel health | Every 30 s and on any state change. |

### Config response (`200`)

```json
{
  "configVersion": 7,
  "kitId": "kit-0001",
  "kitNumber": 1,
  "gatewayName": "wg1.<environment DNS suffix>",
  "equipmentSubnet": "192.168.0.0/24",
  "tunnelSubnet": "10.88.1.0/24",
  "wireguard": { "endpoint": "wg1.<environment DNS suffix>:51820", "hubPublicKey": "<base64 key>", "piAddress": "10.88.1.2/24", "persistentKeepalive": 25 },
  "maxLeaseSeconds": 14400,
  "offlineGraceSeconds": 900
}
```

### Lease object

```json
{
  "leaseId": "lse_01J…",
  "state": "active",
  "labNumber": 2011,
  "labSuffix": "A",
  "peerPublicKey": "<base64 WireGuard key>",
  "tunnelAddress": "10.88.1.12/32",
  "authorizedRoutes": [
    { "dest": "192.168.0.0/24", "protocol": "any" }
  ],
  "notBefore": "2026-10-12T14:00:00Z",
  "expiresAt": "2026-10-12T16:00:00Z"
}
```

Lease rules:

- `state` is `scheduled`, `active`, or `revoked`. Agoge admits the peer on the hub, and the Pi applies its authorized routes, only while `active` and `notBefore ≤ now < expiresAt`.
- The Pi removes the peer's routes at `expiresAt` on its own clock, even if Agoge is unreachable.
- Each `authorizedRoutes` entry must sit inside `equipmentSubnet`; ports are a list of 1–65535, `protocol` is `any, tcp, udp, or icmp (ports only for tcp and udp`). Any route outside the subnet rejects the whole lease (`ack` with `route_not_authorized`).
- The Pi enforces routes per peer with firewall rules; traffic matching no rule is dropped and counted.
- One active lease per `peerPublicKey`; overlapping `tunnelAddress` values reject the later lease (`lease_conflict`).
- If Agoge is unreachable, existing leases continue to expiry; new leases are not installed after `offlineGraceSeconds`.

### Ack body

`{ "result": "applied" | "rejected" | "removed", "code": "<error code or null>", "appliedAt": "<UTC time>" }`

### Error responses

Errors use RFC 9457 problem JSON: `{ "type", "title", "status", "code", "detail", "requestId" }`. The Pi acts on `code`, never on `detail` text.

| HTTP | `code` | Meaning | Pi behaviour |
| --- | --- | --- | --- |
| 400 | `invalid_request` | Malformed request from the Pi | Log; do not retry the same request. |
| 401 | `unauthenticated` | Token missing, wrong, or expired | Stop polling; status `auth_failed`; wait for re-enrollment. |
| 403 | `kit_disabled` | Kit administratively disabled | Remove all peers; poll config every 5 min. |
| 404 | `kit_not_found` / `lease_not_found` | Unknown kit or lease | Kit: treat as `401`. Lease: drop it locally. |
| 409 | `lease_conflict` | Ack or state clashes with Agoge's view | Re-fetch leases, then reconcile. |
| 422 | `route_not_authorized` | Lease route outside equipment subnet | Reject lease; never partially apply. |
| 429 | `rate_limited` | Too many requests | Honour `Retry-After`. |
| 5xx | `server_error` / `maintenance` | Agoge fault | Exponential backoff 5 s → 5 min with jitter; keep current leases. |

## HMI–Pi OPC UA contract

The Pi hosts the OPC UA server and is authoritative; the MTP700 Unified HMI is a client that sends one committed command at a time and displays Pi status.

### Connection

- Endpoint `opc.tcp://192.168.0.5:4840`, security policy `Basic256Sha256`, mode `SignAndEncrypt`.
- HMI authenticates with a dedicated user `hmi` (Proposed) and a trusted application certificate; certificates are exchanged once at kit build.
- Namespace URI `urn:agoge:kit-gateway:v1`; all nodes use string NodeIds `ns=<index of that URI>;s=<name>`. The HMI resolves the index at connect, never hard-codes it.
- HMI subscribes with a 500 ms publishing interval and 250 ms sampling interval.

### Nodes

| Node name (`s=`) | Type | Writer | Meaning |
| --- | --- | --- | --- |
| `Kit.Cmd.LabNumber` | UInt32 | HMI | Requested lab number (encoding below). |
| `Kit.Cmd.LabSuffix` | String | HMI | Requested lab variant suffix. |
| `Kit.Cmd.Code` | UInt16 | HMI | `1` StartLab, `2` StopLab, `3` ResetKit. |
| `Kit.Cmd.Seq` | UInt32 | HMI | Commit trigger; written last. |
| `Kit.Ack.Seq` | UInt32 | Pi | Last command sequence processed. |
| `Kit.Ack.Result` | Int16 | Pi | Result of that command (codes below). |
| `Kit.Heartbeat.Hmi` | UInt32 | HMI | Incremented every 1 s. |
| `Kit.Heartbeat.Pi` | UInt32 | Pi | Incremented every 1 s. |
| `Kit.Status.State` | UInt16 | Pi | `0` Booting, `1` Idle, `2` LabActive, `3` Degraded, `4` Fault. |
| `Kit.Status.ActiveLabNumber` | UInt32 | Pi | `0` when no lab is active. |
| `Kit.Status.ActiveLabSuffix` | String | Pi | Empty when no lab is active. |
| `Kit.Status.LeaseExpiresUtc` | DateTime | Pi | Expiry of the active lease; MinDate when none. |
| `Kit.Status.AgogeLink` | Boolean | Pi | `true` if the last Agoge poll succeeded within 60 s. |
| `Kit.Status.PlcReachable` | Boolean | Pi | `true` if the PLC answered within the last 10 s. |
| `Kit.Status.Message` | String | Pi | Short operator text, max 64 characters. |
| `Kit.Status.UpdatedUtc` | DateTime | Pi | Time the Pi last refreshed any status node. |

The `hmi` user can write only the four `Kit.Cmd.*` nodes and `Kit.Heartbeat.Hmi`; every other node is read-only to it.

### Lab number and suffix

- `LabNumber` is a 32-bit unsigned integer: `major × 1000 + minor`. Lab 2.11 = `2011`; Lab 2.1 = `2001`. Valid range `1001`–`999999`; `0` means “no lab”.
- `LabSuffix` must match `^[A-Z0-9]{0,4}$` (empty allowed). The HMI enforces it with an input mask; the Pi re-validates and is authoritative.
- StartLab succeeds only if an active lease exists with the same `labNumber` and `labSuffix`.

### Committed command sequence

1. HMI checks `Kit.Ack.Seq == Kit.Cmd.Seq` (no command in flight) and that the Pi heartbeat is fresh.
2. HMI writes `LabNumber`, `LabSuffix`, and `Code`.
3. HMI writes `Kit.Cmd.Seq = previous + 1` as a separate, last write. This is the commit.
4. On a `Seq` change, the Pi latches the three payload nodes, validates, acts, then writes `Ack.Result` followed by `Ack.Seq = Cmd.Seq`.
5. HMI reads the result when `Ack.Seq` equals its `Seq`. No ack within 5 s = show “Gateway not responding”; retry only with a new `Seq`.

Sequence rules: `0` is reserved for “never commanded”; after `4294967295` the next value is `1`. A `Seq` that is not exactly `Ack.Seq + 1` is acked with `5`. On Pi restart the Pi sets `Ack.Seq` to the current `Cmd.Seq` and never replays a command.

| `Ack.Result` | Meaning |
| --- | --- |
| `0` | OK |
| `1` | Busy (previous action still running) |
| `2` | Invalid lab number |
| `3` | Invalid suffix |
| `4` | No matching active lease |
| `5` | Out-of-sequence command |
| `6` | Unknown command code |
| `7` | Internal error (see `Status.Message`) |

### Heartbeat and stale-data rules

- Each side increments its heartbeat every 1 s and wraps to 0. A heartbeat unchanged for 5 s is stale.
- Pi heartbeat stale, OPC UA connection lost, or any value with a Bad/Uncertain status code → HMI greys all status fields, shows “Gateway offline”, and disables command buttons.
- `Kit.Status.UpdatedUtc` older than 10 s (Pi clock vs. HMI clock, both NTP-synced) → HMI marks status as stale even if the heartbeat moves.
- HMI heartbeat stale → Pi reports `hmiOnline: false` to Agoge and keeps running labs; it does not stop leases because the panel is down.
- The HMI never caches status across a reconnect; it shows stale until the first fresh subscription update.

## Open decisions and sign-off

The contract is complete once the items below are closed and all three implementers sign; every acceptance criterion already has a section.

| Acceptance criterion | Section | Status (Nov 3, 2026) |
| --- | --- | --- |
| Record PLC, HMI, router, Pi, firmware, software versions | Reference kit versions | Done: every device recorded from the rigs, Oct 16–30. |
| Subnets, stable Pi address, `wg<n>` naming, WireGuard port, API origin | Network and addressing | Done for the MVP. Test values recorded (`app.agoge-labs.com/test-dev`; hub seen at 136.115.55.196 and 35.253.5.173, port `51820`); production names are recorded when production Agoge exists. |
| Authenticated config and lease contract, routes, errors | Agoge–Pi contract | Routes decided (whole kit network); manual setup for now. Follow-up: the Agoge implementer reviews the API section. |
| OPC UA nodes/types, lab number, suffix, command/ack, heartbeat, status, stale data | HMI–Pi OPC UA contract | Written. Follow-up: align with UA Fayetteville's HMI implementation instructions. |
| Implementers can build without unresolved decisions | This section | Both former blockers answered Nov 3. Confirm the MVP defaults, then 3 sign-offs. Tunnel test passed Nov 3: an Agoge lab reached the Pi over WireGuard. |

### Decisions to close

#### Formerly blocking: answered Nov 3, 2026, with follow-ups

- [ ] **HMI role.** The MTP700 runs the Pi's Node-RED dashboard in Firefox (image V20); no WinCC Unified project exists, and the development rig has no HMI fitted. Answered Nov 3, 2026: the lead agreed to the dashboard for now, but UA Fayetteville has provided HMI implementation instructions, so a proper HMI setup is expected soon. Follow-up: compare those instructions with the HMI–Pi OPC UA section and align the two before anyone builds.
- [ ] **Agoge setup method.** Today the gateway is registered by hand in the Agoge web UI and the Pi's `wg0` is configured by hand. Answered Nov 3, 2026: manual setup is fine for now, and the Agoge side can set up the API quickly, which would simplify setup. Follow-up: the Agoge implementer reviews the Agoge–Pi API section and confirms or adjusts it.

#### MVP defaults: accepted at sign-off unless someone objects

- [ ] Kit LAN stays `192.168.0.0/24`. Students reach the kit from Agoge Kali machines, not home networks, so the home-router collision does not apply. Agoge rejects overlapping LANs, so a second kit will need a different range.
- [ ] WireGuard uses UDP `51820` on the hub, as the Agoge test hub does; switch to a fallback such as UDP 443 only if a site blocks it.
- [ ] Every kit Pi syncs time with chrony from internet NTP before it applies any lease. Working on the development Pi since Oct 30; the presentation Pi still needs it.
- [ ] Pi software stays as recorded (Raspbian 11, Node.js 12, Node-RED 1.0.4) for the MVP. Upgrading is a follow-up task, because Node.js 12 no longer gets security fixes.
- [ ] Lab number is `major × 1000 + minor` (Lab 2.11 = `2011`); suffix matches `^[A-Z0-9]{0,4}$`.
- [ ] If the Agoge–Pi API is used, the Pi authenticates with a bearer token; mTLS later.
- [ ] The PLC's OPC UA “No security” and anonymous access is accepted as a known lab risk for the MVP, since students can reach it.

#### Closed

- [x] Inbound reachability: `wg<n>` is a cloud-hosted hub the Pi dials out to (Oct 21, 2026).
- [x] Physical CPU is CPU 1512SP F-1 PN (`6ES7 512-1SM03-0AB0`), matching the TIA project; the slot table in the hardware notes is out of date.
- [x] HMI keeps `192.168.0.13` through a DHCP reservation (Oct 19, 2026).
- [x] Students reach the whole kit network `192.168.0.0/24` through their Agoge Kali machine and the WireGuard tunnel (Oct 27, 2026).
- [x] The development rig's Pi is registered in Agoge test-dev as wireguard-test-01 on 192.168.0.0/24, assigned to a test lab (confirmed Nov 2, 2026).

### Sign-off

| Role | Name | Decision | Date |
| --- | --- | --- | --- |
| Agoge implementer |  |  |  |
| Pi implementer |  |  |  |
| HMI implementer |  |  |  |
