# FFmpeg 01

记录一些乱七八糟的命令。

## FFmpeg 音频音量探测

音频音量与音频波形相关的滤镜操作。

### 音频音量获得

使用 FFmpeg 可以获得音频的音量分贝，以及与音频相关的一些信息，可以使用滤镜 `volumedetect` 获得，例如：

`ffmpeg -i music.mp3 -filter_complex volumedetect -c:v copy -f null /dev/null`

`mean_volume` 为获得的音频的平均大小。

### 绘制音频波形

随着声音分贝的增大，波形波动越强烈，使用 FFmpeg 可以通过 `showwavespic` 滤镜来绘制音频的波形图，例如：

`ffmpeg -i music.mp3 -filter_complex "showwavespic=s=640x120" -frames:v 1 output.png`

命令执行之后将会生成一个宽高为 *640x120* 大小的 output.png 图片，图片内容为音频波形。

绘制不同声道的波形图：

`ffmpeg -i music.mp3 -filter_complex "showwavespic=s=640x240:split_channels=1" -frames:v 1 output.png`

## FFmpeg 视频抠图合并

FFmpeg 可以进行视频抠图与背景视频合并的操作 —— `chromakey` 操作。

**FFmpeg 滤镜 chromakey 参数**

| 参数 | 类型 | 说明 |
|:------:|:------:|:------:|
| color | 颜色 | 设置 chromakey 颜色值，默认为黑色 |
| similarity | 浮点 | 设置 chromakey 相似值 |
| blend | 浮点 | 设置 chromakey 融合值 |
| yuv | 布尔 | yuv 替代 rgb，默认为 false |

示例：

`ffmpeg -i input.mp4 -i input_green.mp4 -filter_complex "[1:v]chromakey=Green:0.1:0.2[ckout];[0:v][ckout]overlay[out]" -map "[out]" output.mp4`

命令行执行之后，会设置 chromakey 的背景色为绿色，设置标签为 ckout，然后将 ckout 铺在以 input.mp4 的视频为背景的画布上，最后输出为 output.mp4。

## FFmpeg 3D 视频处理

**使用黄蓝眼镜观看**

`ffplay -vf "stereo3d=sbsl:aybd" input.mp4`

**使用红蓝眼镜观看**

`ffplay -vf "stereo3d=sbsl:arbg" input.mp4`

*好像并没有什么卵用...*


## FFmpeg 定时视频截图

使用 FFmpeg 截图有很多中，常见的为使用 `vframe` 参数与 `fps` 滤镜。

### vframe 参数截取一张图片

`ffmpeg -i input.mp4 -ss 00:00:7.435 -vframes 1 out.png`

命令行执行之后，FFmpeg 会定位到 input.mp4 的第 7 秒位置，获得对应的视频帧，然后将图像解码成 RGB24 的图像并封装成 PNG 图像。

### fps 滤镜定时获得图片

`ffmpeg -i input.mp4 -vf fps=1 out%d.png`

命令执行之后，将会隔 1 秒钟生成一张 PNG 图片

`ffmpeg -i input.mp4 -vf fps=1/60 out%d.png`

命令执行之后，将会隔 1 分钟生成一张 PNG 图片

`ffmpeg -i input.mp4 -vf "select='eq(pict_type,PICT_TYPE_I)'"  out%04d.png`

命令行执行以后，FFmpeg 将会判断图像类型是否为 I 帧，如果是 I 帧则会生成一张 PNG 图像。

## FFmpeg 对音视频倍速处理

常见的倍速处理方式包含*跳帧播放*与*不跳帧播放*，两种处理方式 FFmpeg 均可支持。

### atempo 音频倍速处理

在 FFmpeg 的音频处理滤镜中，atempo 是用来处理倍速的滤镜，能够控制音频播放速度的快与慢，这个滤镜只有一个参数：`tempo`，，将这个参数的值设置为浮点型，取值范从 0.5 到 2，0.5 则是原来速度的一半，调整为 2 则是原来速度的 2 倍速。

`ffmpeg -i music.mp3 -filter_complex "atempo=tempo=0.5" -acodec aac output.aac`

半速播放

` ffmpeg -i music.mp3 -filter_complex "atempo=tempo=2.0" -acodec aac output.aac`

二倍速播放

### setpts 视频倍速处理

在 FFmpeg 的视频处理滤镜中，通过 setpts 能够控制视频速度的快与慢，这个滤镜只有一个参数：**expr**，这个参数可用来描述视频每一帧的时间戳。

**FFmpeg 滤镜 setpts 参数**

| 值 | 说明 |
|:-----:|:------:|
| FRAME_RATE | 根据帧率设置帧率值，只用于固定帧率 |
| PTS | 输入的 pts 时间戳 |
| PICTIME | 使用 RTC 的时间作为时间戳（即将弃用） |
| TB | 输入的时间戳的时间基 |

`ffmpeg -re -i input.mp4 -filter_complex "setpts=PTS*2" output.mp4`

半速处理

`ffmpeg -re -i input.mp4 -filter_complex "setpts=PTS/2" output.mp4`

二倍速处理
