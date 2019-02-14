# 调整音视频播放速度

*快放*或者*慢放*。

## 调整视频速率

原理：**修改视频的`pts`、`dts`**。

此过程由于不用进行解码编码，所以耗时很少。

### setpts 修改视频速率

`ffmpeg -i demo.mp4 -an -filter:v "setpts=0.5*PTS" output.mp4` 加速，但是分辨率降低了

注意：

* 调整速度倍率范围 [0.25, 4]
* 如果只调整视频的话，最好把音频禁掉
* 对视频进行加速时，如果不想丢帧，可以用 -r 参数指定输出视频 PTS 
	
	`ffmpeg -i demo.mp4 -an -r 60 -filter:v "setpts=2*PTS" output.mp4`
	
## 调整音频速率

原理：**简单的方法是调整音频采样率，但是这种方法会改变音色，一般采用通过对原音进行重采样，差值等方法**。

### atempo filter

`ffmpeg -i demo.mp4 -filter:a "atempo=2.0" -vn output.mp4`

注意：

* 倍率调整范围为 [0.5, 2.0]
* 如果需要调整四倍可采用以下方法：

	`ffmpeg -i demo.mp4 -filter:a "atempo=2.0,atempo=2.0" -vn output.mp4`
	
## 同时调整

`ffmpeg -i demo.mp4 -filter_complex "[0:v]setpts=0.5*PTS[v];[0:a]atempo=2.0[a]" -map "[v]" -map "[a]" output.mp4`

---
END
	
