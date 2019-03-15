# FFmpeg 02 FFmpeg 采集设备

在使用 FFmpeg 作为编码器时，可以使用 FFmpeg 采集本地的音视频采集设备的数据，然后进行编码、封装、传输等操作。

由于，现在在 windows 平台，所以只学习 windows 平台上的设备操作

## FFmpeg 中 Windows 设备操作

Windows 采集设备的主要方式是 `dshow`、`vfwcap`、`gdigrab`，其中 `dshow` 可以用来抓取摄像头、采集卡、麦克风等，`vfwcap` 主要用来采集摄像头类设备，`gdigrab` 则是抓取 Windows 窗口程序。

### FFmpeg 使用 dshow 采集音视频设备

1. 使用 dshow 枚举设备

    `ffmpeg -f dshow -list_devices true -i dummy`

2. 使用 dshow 展示摄像头

    `ffplay -f dshow -video_size 1280*720 -i video="XiaoMi USB 2.0 Webcam"`

    其中，video_size 指定了视频的分辨率，是**摄像头支持采集的分辨率值**，video="XiaoMi USB 2.0 Webcam" 指定了需要采集的摄像头名字。

3. 将摄像头数据保存为 MP4 文件

    `ffmpeg -f dshow -i video="XiaoMi USB 2.0 Webcam" -f dshow -i audio="麦克风 (Realtek High Definition Audio)" capture.mp4`

### FFmpeg 使用 gdigrab 采集窗口

在 Windows 平台，FFmpeg 支持采集基于 gdi 的屏幕采集设备，这个设备同时支持采集显示器的某一块区域，gdigrab 支持的主要参数如表：

| 参数 | 主要作用 |
|:-----:|:------:|
| draw_mouse | 是否绘制采集鼠标指针 |
| show_region | 是否绘制采集的边界 |
| framerate | 设置视频帧率，默认为 25 帧，两个标准值分别是 pal，ntsc |
| video_size | 设置视频分辨率 |
| offset_x | 采集区域偏移 x 个像素 |
| offset_y | 采集区域偏移 y 个像素 |

gdigrab 的输入主要有两种方式：desktop 和 title = window_title，其中 desktop 代表采集整个桌面，而 title = window_title 则是采集标题为 window_title 的窗口。

* 使用 gdigrab 采集整个桌面

    `ffmpeg -f gdigrab -i desktop output.mp4`

* 使用 gdigrap 采集某个窗口

    `ffmpeg -f gdigrab -framerate 6 -i title=ffmpeg output.mp4`
