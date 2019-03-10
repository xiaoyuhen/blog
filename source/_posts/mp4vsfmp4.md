title: Fragmented mp4 与 mp4 对比
date: 2019-03-10 11:15:12
tags:
---

# mp4 格式

MP4 实际代表的含义是 MPEG-4 Part 14。它只是 MPEG 标准中的 14 部分。MP4 主要作用是可以实现快进快放，边下载边播放的效果。他是基于MOV，然后发展成自己相关的格式内容。
MP4 中最基本的单元就是Box，它内部是各个 Box 拼接而成的，这里只会讲解最外层三个重要的 Box 来帮助我们理解 mp4。

![mp4 Box](https://i.loli.net/2019/03/10/5c8481a41fe2c.jpg)

## ftyp Box

主要是 MP4 的说明，告诉解码器它的基本解码版本，兼容格式。一般都是 mp4 的第一个 box。

![ftyp box](https://i.loli.net/2019/03/10/5c848243939c4.jpg)

- major_brand: 推荐兼容性。
- minor_version: 最低兼容版本。
- compatible_brands: mp4 中包含的额外格式，比如，AVC，AAC 等音视频解码格式。

## moov Box

内部有多个 Box，主要包含了视频的一些信息，

- 视频创建时间
- 视频修改时间
- 播放速率
- 视频时长
- 音量大小
- 视频宽高
- 字幕语言
- 声道
- 视频/音频帧位置
- ...

## mdat Box

包含视频媒体数据。mdat中的帧依次存放，每个帧的位置、时间、长度都由moov中的信息指定。

mdat Box 基本上占据了视频大小的 95% 以上，得益于 mp4 边下边播的效果，只要浏览器获取到了部分 mdat box，就可以进行播放。

# fmp4 格式

fmp4 是基于 MPEG-4 Part 12 的流媒体格式。

![fmp4 vs mp4](https://i.loli.net/2019/03/10/5c848231a68d3.jpg)

fmp4 与 mp4 很相似，但也有一些区别。

- fmp4 不需要一个 moov Box 来进行 initialization，fmp4 的 moov Box 只包含了一些 track 信息。
- fmp4 的 视频/音频 metadata 信息与数据都存在一个个 moof、mdat 中，它是一个流式的封装格式。

# fmp4 的应用场景

- fmp4 可以结合 MSE 进行 HTML5 直播。
- fmp4 由一系列的片段组成，如果服务器支持 byte-range 请求，我们可以单独请求部分片段，结合 MSE 进行流式播放。

参考：
https://www.w3.org/TR/mse-byte-stream-format-isobmff/
https://l.web.umkc.edu/lizhu/teaching/2016sp.video-communication/ref/mp4.pdf
