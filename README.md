# FFmpeg

## Useful command parameters

### video envoder
```shell
-c:v
```
Manually specify a video encoder.

#### tldr
```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc output.map4
```
(h264_nvenc == libx264 + NVIDIA video card(Hardware acceleration)

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -preset veryfast output.map4
```
(-preset xxx)
xxx:
- ultrafast(fast encoding but large file size.)
- veryfast
- faster
- fast
- medium(default)
- slow
- slower
- verslow

### constant rate factor

```shell
-crf
```
range: 0(lossless but bigger file)-51(bad buf smaller file)
often use: 19-28

#### tldr

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc output.map4
```

### video filter

```shell
-vf
```

#### tldr

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -vf "scale=1024:576" output.map4

ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -vf "scale=-1:720" output.map4
```

where -1 lets ffmpeg help with the derivation

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -vf "transpose=1" output.map4

ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -vf "scale=256:256, transpose=1" output.map4
```
Rotation

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -vf "crop:400:400:100:100" output.map4
```
crop=w : h : x : y(wide, height, coordinates of the upper left corner)

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -vf "crop=iw/3:ih/3" output.map4
```
iw(input width)

### Video cropping and merging

```shell
-ss <start time> -t <length of time>
```
