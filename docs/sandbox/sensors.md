---
sidebar_label: Sensors
sidebar_position: 4
---

import Tabs from '@theme/Tabs'
import TabItem from '@theme/TabItem'
import { Chip } from '@mui/material'

# Sensors

The cameras and weather station you can use from the Sandbox, and how to read them from a node.

On field nodes, each camera or sensor belongs to exactly one node. The Sandbox is different: the nodes and the cameras sit on the same lab network, so **every Sandbox node can reach every camera**.

:::warning[Camera rule]
Sandbox cameras point at objects, such as a screen or a fish tank, never at people. Please don't move or re-aim them.
:::

## Cameras

| Camera | Type | Address | Web page |
| --- | --- | --- | --- |
| Camera 1 | Reolink | `192.168.4.9` | [http://192.168.4.9](http://192.168.4.9) |
| Camera 2 | Reolink | `192.168.4.10` | [http://192.168.4.10](http://192.168.4.10) |

Both cameras are on the `sage-hack` lab network with fixed (static) IP addresses. The web pages only open from a computer on that network, such as a Sandbox node.

{/* TODO: add what each camera points at and a sample photo from each. */}

### Logging in to a camera

Sandbox users share one user login for the cameras. Ask for it in the Sage Slack (see [Get Help](./get-help)). Don't post it publicly.

{/* TODO: decide where students get the shared camera user password. */}

### Grab a frame

The cameras stream live video over RTSP. From a Sandbox node, save one frame to a picture file. Replace `USER` and `PASSWORD` with the shared camera login:

<Tabs>
  <TabItem value="ffmpeg" label="Command line (ffmpeg)" default>

```bash
ffmpeg -rtsp_transport tcp \
  -i "rtsp://USER:PASSWORD@192.168.4.9:554/h264Preview_01_main" \
  -frames:v 1 frame.jpg
```

  </TabItem>
  <TabItem value="python" label="Python (OpenCV)">

```python
import cv2

url = "rtsp://USER:PASSWORD@192.168.4.9:554/h264Preview_01_main"
cap = cv2.VideoCapture(url)
ok, frame = cap.read()
cap.release()

if ok:
    cv2.imwrite("frame.jpg", frame)
    print("Saved frame.jpg", frame.shape)
else:
    print("Could not read from the camera")
```

  </TabItem>
</Tabs>

`h264Preview_01_main` is the full-quality stream. For a smaller, faster stream, use `h264Preview_01_sub` instead.

{/* TODO: test both examples on a Sandbox node; confirm ffmpeg and OpenCV are installed, or add install steps. */}

## Weather station

An **ES-642 Remote Dust Monitor** from Met One Instruments, set up and managed by the Sage team. Sandbox nodes can read its live data over the network.

**What it measures:**

- Particulate (dust) concentration
- Relative humidity
- Barometric pressure
- Temperature
- Air flow rate
- System status

### How it's connected

![Weather station system architecture](https://github.com/user-attachments/assets/8d698b19-fe44-4cf1-86bc-b2e92509081c)

1. The ES-642 sends its readings over an RS-485 serial cable to an industrial Raspberry Pi (EDA ED-IPC2400).
2. The Raspberry Pi runs [SerialMux](https://github.com/seanshahkarami/serialmux), which shares that serial data over the network on TCP port `20001`.
3. Power and network reach the Raspberry Pi over a single Ethernet cable from the Thor blade, through a PoE injector and splitter.
4. Any Sandbox node can connect to the Raspberry Pi on port `20001` and receive the live data, as if the sensor were plugged in directly.

For the full wiring and setup, see the [weather station setup guide](https://github.com/waggle-sensor/summer-camp-2026/blob/main/RPi-and-Sensors-Setup.md#weather-station-system-setup).

### Read the weather data

Replace `WEATHER_STATION_IP` with the station's address.

{/* TODO: add the weather station Raspberry Pi's IP address. */}

<Tabs>
  <TabItem value="nc" label="Command line (netcat)" default>

```bash
nc -C WEATHER_STATION_IP 20001
```

Data should start streaming in. If nothing appears, press **Enter** a few times until you see a `*` prompt, then type `S 1` to start sampling. Press **Ctrl+C** to quit.

  </TabItem>
  <TabItem value="python" label="Python">

```python
import socket

HOST = "WEATHER_STATION_IP"
PORT = 20001

with socket.create_connection((HOST, PORT), timeout=30) as s:
    f = s.makefile("r", encoding="ascii", errors="replace")
    for _ in range(5):
        print(f.readline().strip())
```

  </TabItem>
</Tabs>

<details>
<summary>Useful ES-642 commands</summary>

Type these at the `*` prompt:

| Command | What it does |
| --- | --- |
| `S 1` | Start sampling |
| `S 0` | Stop sampling |
| `RQ` | Show the current measurement |
| `OP` | Show the operation status |
| `H` or `?` | Show the help menu |
| `Q` or `X` | Leave command mode |

</details>

:::caution[It's a shared instrument]
Everyone in the Sandbox reads the same weather station. Stick to the commands above, and don't run calibration or set-point commands (`CAL`, `RA`, `RF`, `RP`, `RR`, `AV`, `AR`, `SPR`, `SPA`).
:::

## Coming soon

<Chip label="Coming soon" size="small" /> **Smart plugs:** Tasmota smart plugs are being set up with fixed IP addresses.

<details>
<summary>Older camera setup (no longer used)</summary>

Cameras used to be connected through GL.iNet Beryl (GL-MT1300) routers and the Sage WireGuard VPN. The Sandbox cameras no longer work this way, but the guide is still available: [Reolink PTZ camera deployment + WireGuard guide](https://github.com/waggle-sensor/summer-camp-2026/blob/main/Remote-Sensor-Setup.md#reolink-ptz-camera-deployment--wireguard-guide-for-sage).

</details>

---

Stuck? [Get help](./get-help)