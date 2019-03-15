# FFmpeg 03 libavformat api

libavformat 是 FFmpeg 中处理音频、视频以及字幕封装和解封装的通用框架，内置了很多处理多媒体文件的 Muxer 和 Demuxer，它支持如 AVInputFormat 的输入容器和 AVOutputFormat 的输出容器，同时也支持基于网络的一些流媒体协议，如 HTTP、RTSP、RTMP 等。

## 音视频流封装

使用 FFmpeg 的 API 进行封装（Muxing）操作的主要步骤：

1. API 注册
2. 申请 AVFormatContext
3. 申请 AVStream
4. 增加目标容器头信息
5. 写入帧数据

    在 FFmpeg 操作数据包时，均采用写帧操作进行音视频数据包的写入，而每一帧在常规情况下均使用 AVPacket 结构进行音视频数据的存储，AVPacket 结构中包含了 PTS、DTS、Data 等数据。

6. 写容器尾数据

## 音视频文件解封装

1. API 注册
2. 构建 AVFormatContext
3. 查找音视频流信息
4. 读取音视频流

    获得音视频流之后，即可通过 av_read_frame 从 AVFormatContext 中读取音视频流数据包，将音视频流数据包读取出来存储在 AVPackets 中，然后就可以通过对 AVPackets 包进行判断，确定其为音频、视频、字幕数据，最后进行解码，或者进行数据存储。

5. 收尾

## 音视频文件解封装

音视频文件解封装操作是一种格式转换为另一种格式的操作。

1. API 注册
2. 构建输入 AVFormatContext
3. 查找流信息
4. 构建输出 AVFormatContext
5. 申请 AVStream
6. stream 信息的复制
7. 写文件头信息
8. 数据包读取和写入
9. 写文件尾信息
10. 收尾

## avio 内存数据操作

在一些应用场景中需要从内存数据中读取 H.264 数据，然后将 H.264 数据封装为 FLV 格式或 MP4 格式，使用 FFmpeg 的 libavformat 中的 avio 方法即可达到该目的，这样从内存中直接操作数据的方式常常被应用于操作已经编码的视频数据或者音频数据，然后希望将数据通过 FFmpeg 直接封装到文件中。

1. API 注册
2. 读一个文件到内存

    通过 av_file_map 可以将输入的文件中的数据映射到内存 buffer 中。

3. 申请 AVFormatContext
4. 申请 AVIOContext
5. 打开 AVFormatContext
6. 查看音视频流信息
7. 读取帧
