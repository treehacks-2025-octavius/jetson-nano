# jetson-nano

This repository includes code we run on the Jetson Nano for object tracking.

## OBS Streaming Details

Prereqs:
- VLC (unused)
- ffmpeg
- OBS

We'll refer to two devices: the server that streams video and the Jetson Nano, which receives the stream. They should be on the same network. We used a Windows device for the server device.

### Server

1. Install OBS.
2. Install OBS RTSP Server.
3. Identify IP address of the server device (eg. `ipconfig` in bash).
4. Start RTSP server in OBS (Tools -> RSTP Server). Note the target.
5. Select a source to stream.

Some things that may or may not be blocking:
- Disable Windows Firewall. We had to disable for public network.
- Create a rule in Windows Defender Firewall -> Advanced Settings -> Create Rule -> Allow connection on X port.

### Jetson Nano

1. Check the connection at `rtsp://<ip addresss of server device>:<port>/<path>`. This can be done with VLC or ffmpeg, but we only got it working with ffmpeg. You should see the source being streamed from the server.
2. Use `v4l2feedbackloop` to set up the stream as a webcam in a socket. This can be verified using any webcam app (eg. Cheese? but it didn't work for us) or an online webcam test.
