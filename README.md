# FFmpeg

## Overview

- [Resources](#resources)
- [Useful command parameters](#useful-command-parameters)
    - [video envoder](#video-envoder)
    - [constant rate factor](#constant-rate-factor)
    - [video filter](#video-filter)
    - [video cropping](#video-cropping)
    - [video merging](#video-merging)
- [Tips](#tips)
    - [merge mp4](#merge-mp4)
    - [Lossless cropping](#lossless-cropping)
    - [make telegram image strikers](#make-telegram-image-strikers)
    - [create Thumbnails](#create-thumbnails)
    - [Add watermark](#add-watermark)
    - [gif animation conversion](#gif-animation-conversion)
    - [screenshot](#screenshot)

## Resources

[piaoyun/ffmpeg.mdffmpeg 视频合并、格式转换、截图](https://gist.github.com/piaoyun/3f0373c9ce40badd384df37712847e21)

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

-   ultrafast(fast encoding but large file size.)
-   veryfast
-   faster
-   fast
-   medium(default)
-   slow
-   slower
-   verslow

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

### Video cropping

```shell
-ss <start time> -t <length of time> -to <end time>
```

#### tldr

```shell
ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -ss 00:00:03 -t 00:00:05 output.map4

ffmpeg -i 1.mkv -loglevel error -c:v h264_nvenc -ss 00:00:03 -to 00:00:08 output.map4
```

### Video merging

Need to list file names to txt file summary before merging starts
mylist.txt:

```
    file 'clip1.mp4'
    file 'clip2.mp4'
    file 'clip3.mp4'
```

#### tldr

```shell
ffmpeg -f concat -i mylist.txt -c copy output.mp4
```

`-c copy` mern just copy, don't encoding(mp4 -> mp4).

## Tips

### merge mp4

```shell
ffmpeg -i "1.mp4" -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate1.ts
ffmpeg -i "2.mp4" -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate2.ts
ffmpeg -i "3.mp4" -c copy -bsf:v h264_mp4toannexb -f mpegts intermediate3.ts
ffmpeg -i "concat:intermediate1.ts|intermediate2.ts|intermediate3.ts" -c copy -bsf:a aac_adtstoasc "output.mp4"
```

### Lossless cropping

```shell
ffmpeg -ss START -t DURATION -i INPUT -vcodec copy -acodec copy OUTPUT
```

- `vcodec copy`
- `acodec copy`

表示所要使用的视频和音频的编码格式，这里指定为copy表示原样拷贝；

### make telegram image strikers

```shell
ffmpeg -i image_2022-09-18_11-02-56.png -vf scale=512:-1 1.png
```

telegram needs png of 512:-1

### Create Thumbnails

```shell
ffmpeg -i test.mp4 -vf "fps=1/10,scale=-2:720" thumbnail-%03d.jpg
```

-   `fps=1/10` One picture every ten seconds
-   `scale=-2:720` the size of the image

### Add watermark

```shell
ffmpeg -i test.mp4 cat.jpg -filter_complex "overlay=100:100" output.mp4
```

`cat.jpg` is watermark picture
`-filter_complex` is filter
`overlay` overlay cat.jpg to test.mp4
`100:100` is the position of watermark in test.mp4

### gif animation conversion

```shell
ffmpeg -i test.avi -ss 0 -t 3 -filter_complex [0:v]fps=15,scale=-1:256,split[a][b];[a]palettegen[p];[b][p]paletteuse output.gif
```

gif 不太适合太长的视频，所以剪成 3 秒的视频，用过滤器缩放视频和调整帧率
`split[a][b];[a]palettegen[p];[b][p]paletteuse` 是因为 gif256 色的限制，需要创建一个调色板

### screenshot

每隔一秒截一张图

```shell
ffmpeg -i out.mp4 -f image2 -vf fps=fps=1 out%d.png
```

每隔20秒截一张图

```shell
ffmpeg -i out.mp4 -f image2 -vf fps=fps=1/20 out%d.png
```
