![Image](unifiedstreaming-logo-black.jpg?raw=true)
# Live Streaming Trial
## Overview
This project demonstrates the use of [FFmpeg](https://ffmpeg.org/) and [Unified Streaming - Origin Live](http://www.unified-streaming.com/products/unified-origin) to present a Live Adaptive Bitrate presentation.

FFmpeg delivers CMAF tracks to Unified Origin using the [DASH-IF Live Media Ingest Protocol - Interface 1](https://dashif-documents.azurewebsites.net/Ingest/master/DASH-IF-Ingest.html)

For more information about Unified Origin or you have any questions please visit see our [Documentation](http://docs.unified-streaming.com/) or contact us at [support@unified-streaming.com](mailto:support@unified-streaming.com?subject=[GitHub]%20Live%20Streaming%20Trial).
![Image](./live-streaming-trial-image.png?raw=true)


The default track configuration created is below, however encoding parameters can be updated within the [docker-compose.yaml](docker-compose.yaml).
- Video Track 1 - 1280x720 300k AVC 48GOP@25FPS
- Video Track 2 - 480x360 100k AVC 48GOP@25FPS
- Audio Track 1 - 64kbs 48kHz AAC-LC - English language
- Audio Track 2 - 64kbs 48kHz AAC-LC - Dutch language

## Setup

1. Install [Docker](http://docker.io)
2. Install [Docker Compose](http://docs.docker.com/compose/install/)
3. Download this demo's [Compose file](https://github.com/unifiedstreaming/live-streaming-trial/blob/stable/docker-compose.yaml)

## Usage

You need a license key to use this software. To evaluate you can create an account at [Unified Streaming Registration](https://www.unified-streaming.com/licenses/access).

The license key is passed to containers using the *USP_LICENSE_KEY* environment variable.

Start the stack using *docker-compose*:

```bash
#!/bin/sh
export USP_LICENSE_KEY=<your_license_key>
docker-compose up
```

Now the project is running a live stream should be available in all streaming formats at the following URLs:

| Streaming Format | Playout URL |
|------------------|-------------|
| HLS | http://localhost/channel1/test.isml/.m3u8 |
| MPEG-DASH | http://localhost/channel1/channel1.isml/.mpd |


## Example HLS Main Manifest
```
% curl -s http://localhost/channel1/channel1.isml/.m3u8
#EXTM3U
#EXT-X-VERSION:4
## Created with Unified Streaming Platform  (version=1.12.1-28247)

# AUDIO groups
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio-aacl-64",LANGUAGE="nl",NAME="Dutch; Flemish",DEFAULT=YES,AUTOSELECT=YES,CHANNELS="1"
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio-aacl-64",LANGUAGE="en",NAME="English",AUTOSELECT=YES,CHANNELS="1",URI="channel1-audio_eng=64000.m3u8"

# variants
#EXT-X-STREAM-INF:BANDWIDTH=192000,AVERAGE-BANDWIDTH=174000,CODECS="mp4a.40.2,avc1.42C015",RESOLUTION=640x360,FRAME-RATE=25,AUDIO="audio-aacl-64",CLOSED-CAPTIONS=NONE
channel1-audio_dut=64000-video=100000.m3u8
#EXT-X-STREAM-INF:BANDWIDTH=425000,AVERAGE-BANDWIDTH=386000,CODECS="mp4a.40.2,avc1.42C01F",RESOLUTION=1280x720,FRAME-RATE=25,AUDIO="audio-aacl-64",CLOSED-CAPTIONS=NONE
channel1-audio_dut=64000-video=300000.m3u8

# keyframes
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=15000,CODECS="avc1.42C015",RESOLUTION=640x360,URI="keyframes/channel1-video=100000.m3u8"
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=44000,CODECS="avc1.42C01F",RESOLUTION=1280x720,URI="keyframes/channel1-video=300000.m3u8
```

## Example MPEG-DASH Manifest
```xml
% curl -s http://localhost/channel1/channel1.isml/.mpd
<?xml version="1.0" encoding="utf-8"?>
<!-- Created with Unified Streaming Platform  (version=1.12.1-28247) -->
<MPD
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns="urn:mpeg:dash:schema:mpd:2011"
  xsi:schemaLocation="urn:mpeg:dash:schema:mpd:2011 http://standards.iso.org/ittf/PubliclyAvailableStandards/MPEG-DASH_schema_files/DASH-MPD.xsd"
  type="dynamic"
  availabilityStartTime="1970-01-01T00:00:00Z"
  publishTime="2023-01-20T13:56:56.216119Z"
  minimumUpdatePeriod="PT2S"
  timeShiftBufferDepth="PT30M"
  maxSegmentDuration="PT2S"
  minBufferTime="PT10S"
  profiles="urn:mpeg:dash:profile:isoff-live:2011,urn:com:dashif:dash264">
  <Period
    id="1"
    start="PT0S">
    <BaseURL>dash/</BaseURL>
    <AdaptationSet
      id="1"
      group="1"
      contentType="audio"
      lang="nl"
      segmentAlignment="true"
      audioSamplingRate="48000"
      mimeType="audio/mp4"
      codecs="mp4a.40.2"
      startWithSAP="1">
      <AudioChannelConfiguration
        schemeIdUri="urn:mpeg:dash:23003:3:audio_channel_configuration:2011"
        value="1" />
      <Role schemeIdUri="urn:mpeg:dash:role:2011" value="main" />
      <SegmentTemplate
        timescale="48000"
        initialization="channel1-$RepresentationID$.dash"
        media="channel1-$RepresentationID$-$Time$.dash">
        <!-- 2023-01-20T13:56:28.800000Z / 1674222988 - 2023-01-20T13:56:38.400000Z -->
        <SegmentTimeline>
          <S t="80362703462400" d="92160" r="4" />
        </SegmentTimeline>
      </SegmentTemplate>
      <Representation
        id="audio_dut=64000"
        bandwidth="64000">
      </Representation>
    </AdaptationSet>
    <AdaptationSet
      id="2"
      group="1"
      contentType="audio"
      lang="en"
      segmentAlignment="true"
      audioSamplingRate="48000"
      mimeType="audio/mp4"
      codecs="mp4a.40.2"
      startWithSAP="1">
      <AudioChannelConfiguration
        schemeIdUri="urn:mpeg:dash:23003:3:audio_channel_configuration:2011"
        value="1" />
      <Role schemeIdUri="urn:mpeg:dash:role:2011" value="main" />
      <SegmentTemplate
        timescale="48000"
        initialization="channel1-$RepresentationID$.dash"
        media="channel1-$RepresentationID$-$Time$.dash">
        <!-- 2023-01-20T13:56:28.800000Z / 1674222988 - 2023-01-20T13:56:38.400000Z -->
        <SegmentTimeline>
          <S t="80362703462400" d="92160" r="4" />
        </SegmentTimeline>
      </SegmentTemplate>
      <Representation
        id="audio_eng=64000"
        bandwidth="64000">
      </Representation>
    </AdaptationSet>
    <AdaptationSet
      id="3"
      group="2"
      contentType="video"
      par="16:9"
      minBandwidth="100000"
      maxBandwidth="300000"
      maxWidth="1280"
      maxHeight="720"
      segmentAlignment="true"
      frameRate="25"
      mimeType="video/mp4"
      startWithSAP="1">
      <Role schemeIdUri="urn:mpeg:dash:role:2011" value="main" />
      <SegmentTemplate
        timescale="600"
        initialization="channel1-$RepresentationID$.dash"
        media="channel1-$RepresentationID$-$Time$.dash">
        <!-- 2023-01-20T13:56:28.800000Z / 1674222988 - 2023-01-20T13:56:38.400000Z -->
        <SegmentTimeline>
          <S t="1004533793280" d="1152" r="4" />
        </SegmentTimeline>
      </SegmentTemplate>
      <Representation
        id="video=100000"
        bandwidth="100000"
        width="480"
        height="360"
        sar="4:3"
        codecs="avc1.42C015"
        scanType="progressive">
      </Representation>
      <Representation
        id="video=300000"
        bandwidth="300000"
        width="1280"
        height="720"
        sar="1:1"
        codecs="avc1.42C01F"
        scanType="progressive">
      </Representation>
    </AdaptationSet>
  </Period>
  <UTCTiming
    schemeIdUri="urn:mpeg:dash:utc:http-iso:2014"
    value="https://time.akamai.com/?iso" />
</MPD>
```
