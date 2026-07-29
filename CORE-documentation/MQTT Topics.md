# MQTT Topics

Created by: Atit Kharel

# 📡 MQTT Topics Reference — Training Factory Industry 4.0 (24V)

**Broker:** TXT Controller at `192.168.0.10:1883`

**Convention:** 

`f/` = factory data, 

`i/` = incoming to broker (published by TXT), 

`o/` = outgoing from broker (subscribed by TXT), 

`fl/` = local variant (bypasses cloud)

---

## All Topics

| Topic | Direction | Publisher | Subscriber | Description |
| --- | --- | --- | --- | --- |
| `f/i/stock` | PLC → TXT | Node-RED (Pi) | TXT | Warehouse inventory state |
| `f/i/order` | PLC → TXT | Node-RED (Pi) | TXT | Factory state / order status |
| `f/o/order` | TXT → PLC | TXT / External | Node-RED (Pi) | Place a production order |
| `fl/o/nfc/ds` | Pi → TXT | Node-RED / External | TXT | NFC commands (local) |
| `fl/i/nfc/ds` | TXT → Pi | TXT | Node-RED / External | NFC scan results (local) |
| `f/o/nfc/ds` | Cloud → TXT | Cloud | TXT | NFC commands (cloud) |
| `i/broadcast` | TXT → all | TXT | Node-RED (Pi) | TXT heartbeat / identity |
| `o/broadcast` | Pi → TXT | Node-RED (Pi) | TXT | Keep-alive from Pi |
| `i/bme680` | TXT → all | TXT | Node-RED / External | Environmental sensor |
| `i/ldr` | TXT → all | TXT | Node-RED / External | Light / brightness sensor |
| `i/cam` | TXT → all | TXT | Node-RED / External | Camera image (base64) |
| `o/ptu` | Cloud → TXT | Cloud | TXT | Pan-tilt unit control |

---

## 🏭 Factory Control Topics

### `f/o/order` — Place an Order

**Direction:** External / TXT → Node-RED → PLC
**Use:** Triggers a production run for the specified workpiece colour.

```json
{
  "type": "WHITE",
  "ts": "2026-06-01T12:00:00.000Z"
}
```

| Field | Values | Notes |
| --- | --- | --- |
| `type` | `WHITE` `RED` `BLUE` | Must match available stock |
| `ts` | ISO 8601 timestamp | Current UTC time |

> ⚠️ Order buttons on TXT only activate when `f/i/order` state is `WAITING_FOR_ORDER` AND stock count > 0
> 

---

### `f/i/order` — Factory State

**Direction:** PLC → Node-RED → TXT (and subscribers)
**Use:** Read-only. Shows what the factory is currently doing. Published when PLC state changes (event-driven, not periodic).

```json
{
  "state": "WAITING_FOR_ORDER",
  "type": "WHITE",
  "ts": "2026-06-01T12:00:00.000Z"
}
```

### Known Factory States

| State | Meaning |
| --- | --- |
| `WAITING_FOR_ORDER` | Factory idle, ready to accept orders |
| `ORDERED` | Order received, sequence starting |
| `IN_PROCESS` | Workpiece in production |

---

### `f/i/stock` — Warehouse Inventory

**Direction:** PLC → Node-RED → TXT (and subscribers)
**Use:** Current state of all 9 warehouse slots. Drives the stock counters on the TXT display and enables/disables order buttons.

```json
{
  "ts": "2026-06-01T12:00:00.000Z",
  "stockItems": [
    {"workpiece": {"id": "04c34892186580", "state": "RAW", "type": "WHITE"}, "location": "A1"},
    {"workpiece": {"id": "04c34892186581", "state": "RAW", "type": "WHITE"}, "location": "A2"},
    {"workpiece": {"id": "04c34892186582", "state": "RAW", "type": "WHITE"}, "location": "A3"},
    {"workpiece": {"id": "04c34892186583", "state": "RAW", "type": "RED"},   "location": "B1"},
    {"workpiece": {"id": "04c34892186584", "state": "RAW", "type": "RED"},   "location": "B2"},
    {"workpiece": {"id": "04c34892186585", "state": "RAW", "type": "RED"},   "location": "B3"},
    {"workpiece": {"id": "04c34892186586", "state": "RAW", "type": "BLUE"},  "location": "C1"},
    {"workpiece": {"id": "04c34892186587", "state": "RAW", "type": "BLUE"},  "location": "C2"},
    {"workpiece": {"id": "04c34892186588", "state": "RAW", "type": "BLUE"},  "location": "C3"}
  ]
}
```

### Warehouse Layout

| Location | Default Type | Row |
| --- | --- | --- |
| A1, A2, A3 | WHITE | Top |
| B1, B2, B3 | RED | Middle |
| C1, C2, C3 | BLUE | Bottom |

### Workpiece Fields

| Field | Values |
| --- | --- |
| `type` | `WHITE` `RED` `BLUE` `NONE` |
| `state` | `RAW` `PROCESSED`  |
| `id` | NFC UID hex string (7 bytes) |

---

## NFC Topics

### `fl/o/nfc/ds` — NFC Commands (Local)

**Direction:** Node-RED / External → TXT
**Use:** Tell the TXT to perform an NFC operation on whatever puck is near the reader.

### Read UID only

```json
{"ts": "2026-06-01T12:00:00.000Z", "cmd": "read_uid"}
```

### Full read (type + state + history)

```json
{"ts": "2026-06-01T12:00:00.000Z", "cmd": "read"}
```

### Write type and state to tag

```json
{
  "ts": "2026-06-01T12:00:00.000Z",
  "cmd": "write",
  "workpiece": {
    "id": "045bafca341290",
    "type": "WHITE",
    "state": "RAW"
  },
  "history": []
}
```

### Delete tag data

```json
{"ts": "2026-06-01T12:00:00.000Z", "cmd": "delete"}
```

### NFC Command Reference

| `cmd` | Action |
| --- | --- |
| `read_uid` | Read only the NFC UID — no workpiece data |
| `read` | Full read: UID + type + state + timestamp history |
| `write` | Write type, state, history to tag (must include `workpiece` field) |
| `delete` | Erase all data from the tag |

> Commands older than 60 seconds are ignored by the TXT
> 

---

### `fl/i/nfc/ds` — NFC Scan Results (Local)

**Direction:** TXT → Node-RED / External
**Use:** Published after any NFC operation completes. Subscribe to this to see results.

```json
{
  "workpiece": {
    "id": "045bafca341290",
    "state": "RAW",
    "type": "WHITE"
  },
  "history": null,
  "ts": "2026-01-14T15:27:35.986Z"
}
```

---

## Sensor Topics

### `i/bme680` — Environmental Sensor

```json
{
  "ts": "2026-06-01T12:00:00.000Z",
  "t": 23.4,
  "rt": 0,
  "h": 45.2,
  "rh": 0,
  "p": 1013.2,
  "iaq": 50,
  "aq": 1,
  "gr": 0
}
```

| Field | Meaning | Unit |
| --- | --- | --- |
| `t` | Temperature (adjusted) | °C |
| `h` | Humidity | % |
| `p` | Pressure | hPa |
| `iaq` | Indoor Air Quality index | 0–500 |
| `aq` | Accuracy (0=unreliable, 3=calibrated) | — |

---

### `i/ldr` — Light Sensor

```json
{"ts": "2026-06-01T12:00:00.000Z", "br": 72.3, "ldr": 22750}
```

| Field | Meaning |
| --- | --- |
| `br` | Brightness % (0–100) |
| `ldr` | Raw resistance value |

---

### `i/cam` — Camera Image

```json
{"ts": "2026-06-01T12:00:00.000Z", "data": "<base64 encoded JPEG>"}
```

---

## System / Heartbeat Topics

### `i/broadcast` — TXT Heartbeat

**Published by TXT on startup and in response to keep-alive.**

```json
{
  "ts": "2026-06-01T12:00:00.000Z",
  "hardwareId": "TXT-XXXXXXXX",
  "hardwareModel": "TXT 4.0",
  "softwareName": "GatewayPLC",
  "softwareVersion": "1.x.x",
  "message": "init"
}
```

| `message` value | Meaning |
| --- | --- |
| `init` | TXT just started up |
| `keep-alive` | Response to heartbeat ping |

---

## 🔧 Testing with External MQTT Client

### Connect

```
Host: 192.168.0.10
Port: 1883
```

### Useful subscribe patterns

```
f/i/#          → all factory incoming (stock + order state)
fl/i/nfc/ds    → NFC scan results
i/#            → all TXT sensor data
```

### Check factory state

```
Subscribe: f/i/order
Publish to f/o/order: {"type":"WHITE","ts":"2026-06-01T12:00:00.000Z"}
→ watch f/i/order for state change
```

### Read a puck's NFC tag

```
1. Hold puck near TXT NFC reader
2. Publish to fl/o/nfc/ds: {"ts":"2026-06-01T12:00:00.000Z","cmd":"read"}
3. Subscribe to fl/i/nfc/ds for result
```

### Write colour to a blank puck

```
1. Hold puck near NFC reader
2. Publish to fl/o/nfc/ds:
   {"ts":"2026-06-01T12:00:00.000Z","cmd":"write","workpiece":{"id":"<uid>","type":"WHITE","state":"RAW"},"history":[]}
3. Verify with a read command
```