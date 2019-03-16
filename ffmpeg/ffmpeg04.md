# FFmpeg 04 libavcodec api

libavcodec 为音视频的编码/解码提供了通用的框架，它包含了很多编码器和解码器，这些编码器/解码器不仅可以用于音频、视频的处理，还能用于字幕流的处理。

## FFmpeg 旧接口视频解码

1. API 注册
2. 查找解码器
3. 申请 AVCodecContext
4. 同步 AVCodecParameters
5. 打开解码器
6. 帧解码
7. 帧存储
8. 收尾

## FFmpeg 旧接口视频编码

1. API 注册
2. 查找编码器
3. 申请 AVCodecContext
4. 打开编码器
5. 申请帧结构 AVFrame
6. 帧编码
7. 收尾
