# Digital Twin - MQTT Topics

Created by: Atit Kharel

**Source:** Node-RED flow `Digital Twin - MQTT Stream` — polls OPC UA (Siemens PLC @ `192.168.0.1:4840`) every 100ms, republishes to MQTT broker on TXT (`192.168.0.10:1883`).

**Wildcard subscribe:** `fl/i/dt/#`

---

### `fl/i/dt/hbw/pos`

HBW (High Bay Warehouse) horizontal + vertical axis positions.

json

`{
  "ts": "2026-06-18T16:45:00.000Z",
  "station": "hbw",
  "hz_axis_actpos": 1234,
  "hz_axis_targetpos": 1300,
  "hz_axis_posreached": false,
  "vert_axis_actpos": 500,
  "vert_axis_targetpos": 500,
  "vert_axis_posreached": true
}`

| Field | Type | Description |
| --- | --- | --- |
| `ts` | string (ISO 8601) | Timestamp of read |
| `station` | string | Always `"hbw"` |
| `hz_axis_actpos` | int | Horizontal axis actual encoder position |
| `hz_axis_targetpos` | int | Horizontal axis target position |
| `hz_axis_posreached` | bool | True when horizontal axis reached target |
| `vert_axis_actpos` | int | Vertical axis actual encoder position |
| `vert_axis_targetpos` | int | Vertical axis target position |
| `vert_axis_posreached` | bool | True when vertical axis reached target |

---

### `fl/i/dt/vgr/pos`

VGR (Vacuum Gripper Robot) horizontal, vertical, and rotation axis positions.

json

`{
  "ts": "2026-06-18T16:45:00.000Z",
  "station": "vgr",
  "hz_axis_actpos": 800,
  "vert_axis_actpos": 300,
  "rot_axis_actpos": 90,
  "hz_axis_targetpos": 800,
  "vert_axis_targetpos": 300,
  "rot_axis_targetpos": 90,
  "hz_axis_posreached": true,
  "vert_axis_posreached": true,
  "rot_axis_posreached": true
}`

| Field | Type | Description |
| --- | --- | --- |
| `ts` | string (ISO 8601) | Timestamp of read |
| `station` | string | Always `"vgr"` |
| `hz_axis_actpos` | int | Horizontal axis actual position |
| `vert_axis_actpos` | int | Vertical axis actual position |
| `rot_axis_actpos` | int | Rotation axis actual position |
| `hz_axis_targetpos` | int | Horizontal axis target position |
| `vert_axis_targetpos` | int | Vertical axis target position |
| `rot_axis_targetpos` | int | Rotation axis target position |
| `hz_axis_posreached` | bool | True when horizontal axis reached target |
| `vert_axis_posreached` | bool | True when vertical axis reached target |
| `rot_axis_posreached` | bool | True when rotation axis reached target |

---

### `fl/i/dt/ssc/pos`

SSC (Sensor/Camera arm) horizontal + vertical axis positions.

json

`{
  "ts": "2026-06-18T16:45:00.000Z",
  "station": "ssc",
  "hz_axis_actpos": 400,
  "hz_axis_targetpos": 400,
  "hz_axis_posreached": true,
  "vert_axis_actpos": 150,
  "vert_axis_targetpos": 150,
  "vert_axis_posreached": true
}`

| Field | Type | Description |
| --- | --- | --- |
| `ts` | string (ISO 8601) | Timestamp of read |
| `station` | string | Always `"ssc"` |
| `hz_axis_actpos` | int | Horizontal axis actual position |
| `hz_axis_targetpos` | int | Horizontal axis target position |
| `hz_axis_posreached` | bool | True when horizontal axis reached target |
| `vert_axis_actpos` | int | Vertical axis actual position |
| `vert_axis_targetpos` | int | Vertical axis target position |
| `vert_axis_posreached` | bool | True when vertical axis reached target |