# Cut and Splice Video files in ffmpeg

## Documentation

[FFmpeg documentation](https://ffmpeg.org/documentation.html)

[FFmpeg manual](https://ffmpeg.org/ffmpeg.html) (command line options)

[FFmpeg Codecs](https://ffmpeg.org/ffmpeg-codecs.html) (libx264 for mp4 video, aac for audio, etc.)

[FFmpeg Filters](https://ffmpeg.org/ffmpeg-filters.html) (-vf and -va filtering options)

[FFmpeg Formats](https://ffmpeg.org/ffmpeg-formats.html) (-movflags +faststart options for muxer, also demuxer options)

## Show info and statistics
The **ffprobe** utility is included when ffmpeg is installed.

```
ffprobe -i input.mp4
```

Info in JSON format:

```
ffprobe -v quiet -print_format json -show_streams -show_format input.mp4
```

## Losslessly cut an mp4 video into several smaller clips

```
ffmpeg -i Walkthrough-20260807_140349-Meeting\ Recording.mp4 \
-ss 00:14:25 -to 00:21:45 -c copy part-1-Emergency-Building-Coordinator-Portal-EBC-stories.mp4 \
-ss 00:21:46 -to 00:28:45 -c copy part-2-Minor-Protection-App-stories.mp4 \
-ss 00:28:49 -to 00:35:00 -c copy part-3-Litigation-Site-stories.mp4 \
-ss 00:39:15 -to 00:41:10 -c copy part-4-Waiver-Lookup-stories.mp4 \
-ss 00:41:55 -to 00:42:49 -c copy part-5-Incident-Site-and-Waiver-demo-later.mp4 \
-ss 00:42:50 -to 00:52:15 -c copy part-6-Risk-Styles-and-other-stories.mp4
```

## Losslessly combine several mp4 video clips into one

1. Create a text file named **file_list.txt** in the same folder as videos.
2. Add each file path on a separate line using the format: **file 'filename.mp4'**.
3. Save and close the text file.
4. Open the terminal in that folder.
4. Run the command: **ffmpeg -f concat -safe 0 -i file_list.txt -c copy output.mp4**

### file_list.txt

```
file 'part-1-Emergency-Building-Coordinator-Portal-EBC-stories.mp4'
file 'part-2-Minor-Protection-App-stories.mp4'
file 'part-3-Litigation-Site-stories.mp4'
file 'part-4-Waiver-Lookup-stories.mp4'
file 'part-5-Incident-Site-and-Waiver-demo-later.mp4'
file 'part-6-Risk-Styles-and-other-stories.mp4'
```

### Run ffmpeg command to assemble files

```
ffmpeg -f concat -safe 0 -i file_list.txt -c copy output.mp4
```

### Transcode from Quicktime .mov to .mp4
See [Convert MOV to MP4 Using FFmpeg Simplified](https://ottverse.com/convert-mov-to-mp4-using-ffmpeg/)
```
ffmpeg -i input.mov -c:v libx264 -crf 20 -c:a aac -b:a 192k -vf format=yuv420p -movflags +faststart output.mp4
```
#### Command Breakdown
Each option in this command serves a specific purpose to ensure high quality and universal playback compatibility:

- **-i input.mov**: Specifies your source QuickTime video file. [[main options](https://ffmpeg.org/ffmpeg.html#Main-options)]
- **-c:v libx264**: Re-encodes (-c or -codec) the *video* stream (-c:v or -codec:v) into H.264 format using the libx264 codec, which plays on almost any device. -c:v sets the video encoder, -c:a sets the audio encoder, and -c:s sets the subtitle encoder. [[main options](https://ffmpeg.org/ffmpeg.html#Main-options), [streamcopy](https://ffmpeg.org/ffmpeg.html#Streamcopy), [stream handling](https://ffmpeg.org/ffmpeg.html#Stream-handling), [FFmpeg Codecs](https://ffmpeg.org/ffmpeg-codecs.html), [libx264 video encoder options](https://ffmpeg.org/ffmpeg-codecs.html#libx264_002c-libx264rgb)]
- **-crf 20**: Sets the Constant Rate Factor for video quality. Lower numbers mean higher quality; 18 to 28 is considered the sweet spot for visual transparency. 0 is lossless (massive file size) and 51 is very high compression (low quality, small files). [[libx264 video encoder options](https://ffmpeg.org/ffmpeg-codecs.html#libx264_002c-libx264rgb)]
- **-c:a aac -b:a 192k**: Use the AAC audio encoder (aac), activate Constant Bit Rate mode and encode at a high-quality 192 kilobits per second (-b:a 192k). [[aac audio encoder options](https://ffmpeg.org/ffmpeg-codecs.html#aac)]
- **-vf format=yuv420p**: Set video filtergraph to enforce the standard YUV 4:2:0 planar 8-bit pixel format. QuickTime files often use pixel formats that default players (like standard Windows or web players) cannot read; this guarantees compatibility. The -vf option is an alias for -filter:v (video) and -va is an alias for -filter:a (audio). [[Filtering](https://ffmpeg.org/ffmpeg.html#Filtering), [FFmpeg Filters](https://ffmpeg.org/ffmpeg-filters.html)]
- **-movflags +faststart**: Moves the file metadata to the front of the container. This allows the video to start playing immediately without waiting for the entire file to download when hosted on the web. [[FFmpeg Formats](https://ffmpeg.org/ffmpeg-formats.html), [MOV/MPEG-4 muxers](https://ffmpeg.org/ffmpeg-formats.html#MOV_002fMPEG_002d4_002fISOMBFF-muxers), [MOV/MPEG-4 muxer options](https://ffmpeg.org/ffmpeg-formats.html#Options-6)]

more stuff...
