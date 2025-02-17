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
```
sudo apt update
sudo apt install v4l2loopback-utils ffmpeg

# create a virtual device
sudo modprobe v4l2loopback devices=1 video_nr=10 card_label="Virtual Camera" exclusive_caps=1

# forward RTSP to virtual camera
ffmpeg -i rtsp://your_rtsp_stream_url -f v4l2 -vcodec rawvideo -pix_fmt yuv420p /dev/video10

# verify
ffplay /dev/video10
```

Notes
- If the virtual camera isn't working after something (eg. computer went to sleep), you may need to reset it somehow. Restarting the computer works, but maybe a less drastic option exists?
- The resolution specified when running the nano owl thing must match the output resolution of OBS.
- Server and Jetson have to be on the same wifi!!! SAME WIFI!!!
