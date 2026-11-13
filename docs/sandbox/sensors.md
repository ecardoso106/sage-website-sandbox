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
Sandbox cameras point at objects, such as a monitor or a poster, never at people. Please don't move or re-aim them.
:::

## Cameras

| Camera | Stream address | What it shows |
| --- | --- | --- |
| `evl-cam-3` | `rtsp://media/evl-cam-3` | The bottom edge of an LG monitor and the cables under it (grayscale) |
| `evl-cam-4` | `rtsp://media/evl-cam-4` | A research poster on edge audio sensing, with headphones in front (grayscale) |
| `test-cam` | `rtsp://media/test-cam` | A virtual camera: a color test pattern with a steady 1 kHz tone |

The cameras stream live video over RTSP through a media server on the lab network. Each camera has a short, friendly name, and you don't need a username or password to read its stream. The addresses only work from a Sandbox node.

`test-cam` isn't a real camera. It makes a test pattern and tone on demand, so it's a good way to check your code before you use the real cameras.

:::note[Audio]
Every stream has an audio track, but only `test-cam` has sound. The `evl-cam-3` and `evl-cam-4` audio tracks are silent.
:::

The camera list and where the cameras point may change. This page will be updated when they do.

{/* TODO: add a sample photo from each camera. A camera/mic list with descriptions (cameras.txt) may be added on the nodes; link it here when it exists. */}

### Grab a frame

From a Sandbox node, save one frame from each camera to a picture file:

<Tabs>
  <TabItem value="ffmpeg" label="Command line (ffmpeg)" default>

```bash
ffmpeg -i rtsp://media/evl-cam-3 -frames:v 1 evl-cam-3.jpg
ffmpeg -i rtsp://media/evl-cam-4 -frames:v 1 evl-cam-4.jpg
```

  </TabItem>
  <TabItem value="python" label="Python (OpenCV)">

```python
import cv2

url = "rtsp://media/evl-cam-3"
cap = cv2.VideoCapture(url)
ok, frame = cap.read()
cap.release()

if ok:
    cv2.imwrite("evl-cam-3.jpg", frame)
    print("Saved evl-cam-3.jpg", frame.shape)
else:
    print("Could not read from the camera")
```

  </TabItem>
</Tabs>

Swap in `evl-cam-4` or `test-cam` to use another stream.

{/* TODO: test the OpenCV example on a Sandbox node (the ffmpeg commands were tested on H01D). */}

### Record a short clip

Save a 3-second video, with its audio, from each stream:

```bash
ffmpeg -i rtsp://media/evl-cam-3 -t 3 evl-cam-3.mp4
ffmpeg -i rtsp://media/evl-cam-4 -t 3 evl-cam-4.mp4
ffmpeg -i rtsp://media/test-cam -t 3 test-cam.mp4
```

`-t 3` sets the length in seconds. `test-cam.mp4` will have the 1 kHz tone, so you can use it to check that your audio works too.

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