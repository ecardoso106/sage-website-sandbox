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

| Camera | Stream address |
| --- | --- |
| `evl-3` | `rtsp://media/camera/evl-3` |
| `evl-4` | `rtsp://media/camera/evl-4` |

The cameras stream live video over RTSP through a media server on the lab network. Each camera has a short, friendly name, and you don't need a username or password to read its stream. The addresses only work from a Sandbox node.

{/* TODO: add what each camera points at and a sample photo from each. */}

### Grab a frame

From a Sandbox node, save one frame from each camera to a picture file:

<Tabs>
  <TabItem value="ffmpeg" label="Command line (ffmpeg)" default>

```bash
mkdir -p cameras
ffmpeg -i rtsp://media/camera/evl-3 -frames:v 1 cameras/evl-3.jpg
ffmpeg -i rtsp://media/camera/evl-4 -frames:v 1 cameras/evl-4.jpg
```

  </TabItem>
  <TabItem value="python" label="Python (OpenCV)">

```python
import cv2

url = "rtsp://media/camera/evl-3"
cap = cv2.VideoCapture(url)
ok, frame = cap.read()
cap.release()

if ok:
    cv2.imwrite("evl-3.jpg", frame)
    print("Saved evl-3.jpg", frame.shape)
else:
    print("Could not read from the camera")
```

  </TabItem>
</Tabs>

To grab from the other camera, change `evl-3` to `evl-4`.

{/* TODO: test the OpenCV example on a Sandbox node (the ffmpeg commands were tested on H01D). */}

## Weather station <Chip label="Coming soon" size="small" />

An **ES-642 Remote Dust Monitor** from Met One Instruments, set up and managed by the Sage team. It isn't available to Sandbox users yet. Once it is, Sandbox nodes will be able to read its live data over the network.

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
3. Power and network reach the Raspberry Pi over a single Ethernet cable from the Thor-blade, through a PoE injector and splitter.
4. Any Sandbox node will be able to connect to the Raspberry Pi on port `20001` and receive the live data, as if the sensor were plugged in directly.

For the full wiring and setup, see the [weather station setup guide](https://github.com/waggle-sensor/summer-camp-2026/blob/main/RPi-and-Sensors-Setup.md#weather-station-system-setup).

### Read the weather data

<Chip label="Coming soon" size="small" /> Instructions for reading the weather station from a Sandbox node will be added here once it's available.

## Coming soon

<Chip label="Coming soon" size="small" /> **Smart plugs:** Tasmota smart plugs are being set up with fixed IP addresses.

<details>
<summary>Older camera setup (no longer used)</summary>

Cameras used to be connected through GL.iNet Beryl (GL-MT1300) routers and the Sage WireGuard VPN. The Sandbox cameras no longer work this way, but the guide is still available: [Reolink PTZ camera deployment + WireGuard guide](https://github.com/waggle-sensor/summer-camp-2026/blob/main/Remote-Sensor-Setup.md#reolink-ptz-camera-deployment--wireguard-guide-for-sage).

</details>

---

Stuck? [Get help](./get-help)