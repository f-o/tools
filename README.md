<div align="center">

<img src="toolbox.png" alt="toolbox" width="200">

# Toolbox

This is a collection of tools, scripts, and other types of code, that I use for my daily work.<br>
It is meant to be a quick way for me to remember certain commands. I hope you find them useful too.

<br>
<br>
</div>


# Table of Contents

- [Table of Contents](#table-of-contents)
- [Linux](#linux)
    - [Folder Disk Usage](#folder-disk-usage)
    - [Get Public IP Address](#get-public-ip-address)
    - [Upload with Upsies](#upload-with-upsies)
- [Python](#python)
- [FFmpeg](#ffmpeg)
    - [MKV to WAV](#mkv-to-wav)
    - [Export SRT from MKV](#export-srt-from-mkv)
    - [Split video into 15 minute parts](#split-video-into-15-minute-parts)
    - [Trim video between timestamps](#trim-video-between-timestamps)

---

# Linux

## Folder Disk Usage

To calculate the total storage of all folders in the current directory, you can use the `du` (disk usage) command. Here are two useful variations:

### 1. Display Disk Usage with Folder Sizes

The following command shows the disk usage of each folder in the current directory, including subdirectories up to one level deep:

```bash
du -h --max-depth=1
```

- `du`: Disk usage command.
- `-h`: Human-readable format (e.g., KB, MB, GB).
- `--max-depth=1`: Limits the display to the immediate folders in the current directory.

### 2. Display and Sort Disk Usage by Size

To sort the output by folder size in a human-readable format, you can use the following command:

```bash
du -h --max-depth=1 | sort -h
```

- `sort -h`: Sorts the output based on human-readable sizes.

This command not only shows the disk usage of each folder but also arranges them in ascending order based on size. Adjust the `--max-depth` option as needed to control the depth of the directory tree included in the output.


## Get Public IP Address

To retrieve your public IP address from the command line using `curl`, you can use the following command:

```bash
curl ipinfo.io
```

This command sends a GET request to ipinfo.io and prints the response, which contains your public IP address, as well as information about your ISP.<br>

Sample output:
```json
{
  "ip": "212.x.x.x",
  "city": "Amsterdam",
  "region": "North Holland",
  "country": "NL",
  "loc": "52.3740,4.8897",
  "org": "AS60781 LeaseWeb Netherlands B.V.",
  "postal": "1012",
  "timezone": "Europe/Amsterdam",
  "readme": "https://ipinfo.io/missingauth"
}
```

## Upload with Upsies

Base command to upload a folder using [Upsies](https://upsies.readthedocs.io/en/stable/).

```bash
upsies submit TRACKER PATH
```
- `submit`: Generate all required metadata and upload to TRACKER.
- `TRACKER`: Tracker shortcode (BHD).
- `PATH`: Path to the folder to upload.

Using debug and ignore cache
```bash
upsies --debug debug.log -C submit TRACKER PATH
```
- `--debug debug.log`: Enable debug logging to a file.
- `-C`: Ignore results from previous calls (cache).


# FFmpeg

## MKV to WAV
```
ffmpeg -i input.mkv -vn -acodec pcm_s16le output.wav
```
- `-i input.mkv`: Specifies the input file (replace "input.mkv" with the actual filename and path of your MKV file).
- `-vn`: This option tells FFmpeg not to include video in the output.
- `-acodec pcm_s16le`: This sets the audio codec to PCM (Pulse Code Modulation) with 16-bit signed little-endian samples, which is the format commonly used in WAV files.
- `output.wav`: Specifies the output file name and format.

Only first 30 seconds
```
ffmpeg -i input.mkv -vn -acodec pcm_s16le -t 30 output.wav
```
- `-t 30`: Specifies the duration for the conversion. In this case, it's set to 30 seconds. 


## Export SRT from MKV

```
ffmpeg -i input.mkv -map 0:s:0 output.srt
```
- `-i input.mkv`: Specifies the input MKV file (replace "input.mkv" with the actual filename and path of your MKV file).
- `-map 0:s:0`: This option selects the first subtitle stream from the input file. If your MKV file has multiple subtitle streams and you want a different one, you may need to adjust the index accordingly (e.g., 0:s:1 for the second subtitle stream).
- `output.srt`: Specifies the output file name and format (replace "output.srt" with your desired filename).

## Split video into 15 minute parts

```
ffmpeg -i input.mp4 -c copy -map 0 -f segment -segment_time 900 -reset_timestamps 1 output_%03d.mp4
```
- `-i input.mp4`: Specifies the input video file (replace "input.mp4" with the actual filename and path of your video).
- `-c copy`: This option tells FFmpeg to copy the video and audio streams without re-encoding. It's a faster operation and doesn't result in any loss of quality.
- `-map 0`: Includes all streams from the input file in the output.
- `-f segment`: Specifies the use of the segment muxer, which is used for segmenting the input into chunks.
- `-segment_time 900`: Sets the segment duration to 900 seconds (15 minutes). You can adjust this value to your desired chunk duration.
- `-reset_timestamps 1`: Ensures that each segment starts with a timestamp of 0.

The input needs to start from 00:05
```
ffmpeg -ss 00:05:00 -i input.mp4 -c copy -map 0 -f segment -segment_time 900 -reset_timestamps 1 output_%03d.mp4
```
- `-ss 00:05:00`: Sets the start time of the input video to 5 minutes. This means the first segment will start from 00:05:00.

## Trim video between timestamps
```
ffmpeg -i input.mp4 -ss 00:00:30 -to 00:01:00 -c copy output.mp4
```
- `-i input.mp4`: Specifies the input video file (replace "input.mp4" with the actual filename and path of your video).
- `-ss 00:00:30`: Sets the start time to 30 seconds.
- `-to 00:01:00`: Sets the end time to 1 minute.
- `-c copy`: Copies the video and audio streams without re-encoding, ensuring there is no loss of quality.
- `output.mp4`: Specifies the output file name and format.

Using duration
```
ffmpeg -i input.mp4 -ss 00:00:30 -t 30 -c copy output.mp4
```
- `-t 30`: Sets the duration to 30 seconds, so the output will be from 00:00:30 to 00:01:00.