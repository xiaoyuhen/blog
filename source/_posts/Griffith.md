title: Griffith - 知乎视频播放器
date: 2019-04-05 19:44:46
tags:
---

![Griffith](https://cdn-images-1.medium.com/max/1600/1*Ra5lZXCyVUufV1WUCj88Rw.jpeg)

# 知乎视频播放器 Griffith 开源介绍

## Griffith 是什么？

Griffith 是一个基于 React 的视频播放器，目前已在知乎 web 和 mobile web 内使用，并在 GitHub 上开源。

## 开源地址及示例

GitHub 地址：https://github.com/zhihu/griffith

CodeSandbox 示例：https://codesandbox.io/s/74olr5z02x

## 特性
### 简洁易用的 UI
Griffith 提供了简洁易用的播放器 UI。目前知乎网页端视频播放器就是使用的 Griffith。

![Griffith](https://zhstatic.zhihu.com/cfe/griffith/player.png)

### 快捷键支持

Griffith 参考 YouTube 进行了快捷键支持，后续还会添加更多的快捷键。

- 空格键：进度条处于选中状态时，可控制视频的播放/暂停。如果已经选中某个按钮，则可用于点击该按钮。
- K： 在播放器中暂停/播放视频。
- 选中进度条状态下的向左/向右箭头：快进/快退 5 秒钟。
- J：在播放器中快退 10 秒。
- L：在播放器中快进 10 秒。
- 选中进度条状态下的向上/向下箭头：将音量增大/减小 5%。
- 选中进度条状态下的数字 1 到 9（不是数字小键盘上的数字）：跳至视频进度的 10% 到 90%。
- 选中进度条状态下的数字 0（不是数字小键盘上的 0）：跳至视频的开头。
- F：启用全屏模式。如果已启用全屏模式，则再次按 F 键或按 Esc 键可退出全屏模式。
- M：切换静音。

### React-friendly

Griffith 是一个基于 React 开发的 web 视频播放器。对于 React 应用，可以直接通过组件调用的方式使用。

Griffith 使用 Context API 进行状态管理。对于 React 应用，可以通过引入 Griffith 的 Context 来实现弹幕等自定义功能。

### 免构建

对于非 React 应用，或者不愿意通过 npm 包安装的用户，Griffith 提供 standalone 模式可以免构建工具直接在浏览器中使用。

### 丰富的事件系统

Griffith 定义了丰富的事件系统。

对于视频播放器中常见的首帧时长，缓冲次数等指标，可以通过接收 Griffith 事件来进行打点记录。

对于一些需要与播放器进行通信的功能，可以通过往 Griffith 发送事件来与播放器进行交互。

### 流式播放
Griffith 使用了 Media Source Extensions™ ，支持对 mp4 和 m3u8 格式的视频进行流式播放。

- 预加载策略： Griffith 可以通过 MSE 动态控制视频加载进度，以达到节省视频 CDN 带宽等目地。
- 动态平滑切换清晰度：Griffith 可以通过 MSE 实现动态平滑切换视频清晰度。

## 如何使用
###React 应用

```js
import Player from 'griffith'

const sources = {
  hd: {
    play_url: 'https://zhstatic.zhihu.com/cfe/griffith/zhihu2018hd.mp4',
  },
  sd: {
    play_url: 'https://zhstatic.zhihu.com/cfe/griffith/zhihu2018sd.mp4',
  },
}

render(<Player sources={sources} />)
```

### standalone 模式
```js
<div id="player"></div>
<script src="https://unpkg.com/griffith-standalone/dist/index.umd.min.js"></script>
<script>
  const target = document.getElementById('player')

  const sources = {
    hd: {
      play_url: 'https://zhstatic.zhihu.com/cfe/griffith/zhihu2018hd.mp4',
    } ,
    sd: {
      play_url: 'https://zhstatic.zhihu.com/cfe/griffith/zhihu2018sd.mp4',
    },
  }

  // 创建播放器
  const player = Griffith.createPlayer(target)

  // 载入视频
  player.render({sources})

  // 销毁视频
  player.dispose()
</script>
```

## 技术细节
- 使用 Yarn workspace 和 Lerna 进行多包管理。
- 使用 rollup 和 webpack 进行打包。
- 使用 new Context API 进行状态管理。
- 使用 CSS-in-JS 方案来管理样式。
- 使用 Jest 来进行单元测试编写。
- 使用 Prettier 进行代码格式统一。
- 使用 hlsjs 来实现流式播放 m3u8 格式视频。
- 使用 griffith-mp4 进行 mp4 到 fmp4 格式的转换并通过 MSE 实现流式播放。

## 结语

Griffith 所有的工作都会在 GitHub 上进行（知乎内部使用的也是同一个仓库）。如果有任何相关的疑问和 bug，欢迎在 GitHub 提交 issue、PR 帮助 Griffith 变得更好。
