![Image](unifiedstreaming-logo-black.jpg?raw=true)
# Live Streaming Trial
## Overview
Launch a live channel and stream just-in-time to any internet connected device from a unified origin.

This trial demonstrates the use of [FFmpeg](https://ffmpeg.org/) and [Unified Streaming - Origin Live](http://www.unified-streaming.com/products/unified-origin) to present a Live Adaptive Bitrate presentation.

FFmpeg delivers CMAF tracks to Unified Origin using the [DASH-IF Live Media
Ingest Protocol - Interface
1](https://dashif-documents.azurewebsites.net/Ingest/master/DASH-IF-Ingest.html)

![Image](./live-streaming-trial-image.png?raw=true)

### What to expect from this trial

Follow the steps below to launch a live channel in less than 30 minutes.

Your 7 day trial key is present in Step 1.


## Prerequisites
Docker, if not already installed see: https://docs.docker.com/get-docker/

Internet access on host through ports 53 and 80; needed to check license key

## Step 1
Start by cloning the Live streaming trial from GitHub and starting the Docker Compose stack:

```
git clone https://github.com/unifiedstreaming/live-streaming-trial.git

cd live-streaming-trial

export UspLicenseKey=<your_license_key>

docker compose up -d
```
## Step 2
Wait a minute or two for all the Docker images to download and the services to start, you can view the status by checking the logs with:

```
docker compose logs
```

And checking the origin is available by querying it with curl:

```
curl http://localhost/channel1/channel1.isml/state
```

Which should respond:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- Created with Unified Streaming Platform  (version=1.12.1-28247) -->
<smil
  xmlns="http://www.w3.org/2001/SMIL20/Language">
  <head>
    <meta
      name="updated"
      content="2023-01-20T15:38:30.557813Z">
    </meta>
    <meta
      name="state"
      content="started">
    </meta>
  </head>
</smil>
```
## Step 3
Play the live stream from host running container:

* Open [DASH stream (http://localhost/channel1/channel1.isml/.mpd)](https://shaka-player-demo.appspot.com/demo/#audiolang=en-GB;textlang=en-GB;uilang=en-GB;asset=http://localhost/channel1/channel1.isml/.mpd;panel=CUSTOM%20CONTENT;build=uncompiled) in latest shaka player
* Open [HLS TS stream (http://localhost/channel1/channel1.isml/.m3u8)](https://hls-js.netlify.app/demo/?src=http://localhost/channel1/channel1.isml/.m3u8) in latest hls.js
* Open [HLS CMAF stream (http://localhost/channel1/channel1.isml/.m3u8?hls_fmp4)](https://hls-js.netlify.app/demo/?src=http://localhost/channel1/channel1.isml/.m3u8?hls_fmp4) in latest hls.js

> **_NOTE:_**
The FFmpeg container is configured to encode multiple video and audio tracks in
realtime. Therefore buffering or stalled experienced when playing the stream
from Unified Origin is subject to the performance of the FFmpeg container. If issues persists, please follow step 4.

## Step 4
Stop the services by running:

```
docker compose down
```
Restart the trial with a simplified example configured to ingest only 1 video and 1 audio track.

```
docker compose -f docker-compose-simple.yaml up -d 
```
Then following Step 3 to playback the new stream.

### Tips
To check when your license key expires: 
```
docker exec -it live-streaming-trial_live-streaming-origin_1 mp4split
--show_license
```

To print and tail origin container's logs: 
```
docker logs -f live-streaming-trial_live-streaming-origin_1
```
To get into origin container's shell: 
```
docker exec -it -w /var/www/unified-origin live-streaming-trial_live-streaming-origin_1 /bin/sh
```

## What's next?
[Learn more about the key features and benefits of using Unified Origin for live streaming](https://docs.unified-streaming.com/documentation/live/index.html)

or

[Contact us](mailto:%20sales@unified-streaming.com) to purchase a license