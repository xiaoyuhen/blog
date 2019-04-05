title: Griffith - A React-based web video player that makes streaming easy
date: 2019-04-05 19:42:41
tags:
---

![Griffith](https://cdn-images-1.medium.com/max/1600/1*Ra5lZXCyVUufV1WUCj88Rw.jpeg)

# What is Griffith?

Griffith is a React-based web video player that has been open sourced on GitHub.

# Code and examples

GitHub: https://github.com/zhihu/griffith

CodeSandbox example: https://codesandbox.io/s/74olr5z02x

# Features
## Simple and easy to use UI
Griffith provides a simple and easy to use player UI.

![Griffith](https://zhstatic.zhihu.com/cfe/griffith/player.png)

## Shortcut support

Griffith refers to YouTube for shortcut key support and will add more shortcuts later.

- Spacebar: Play/Pause when the seek bar is selected. Activate a button if a button has focus.
- k: Pause/Play in player.
- Left/Right arrow on the seek bar: Seek backward/forward 5 seconds.
- j: Seek backward 10 seconds in player.
- l: Seek forward 10 seconds in player.
- Up/Down arrow on the seek bar: Increase/Decrease volume 5%.
- Numbers 1 to 9 on the seek bar (not on the numeric pad): Seek to the 10% to 90% of the video.
- Number 0 on the seek bar (not on the numeric pad): Seek to the beginning of the video.
- f: Activate full screen. If full screen mode is enabled, activate F again or press escape to exit full screen mode.
- m: Toggle mute

## React-friendly

Griffith is a web video player based on React. For React applications, you can use them directly by import components.

Griffith uses the Context API for state management. For React applications, you can implement custom functionality by import Griffith's Context.

## umd

For non-React applications, or users who are unwilling to install through the npm package, Griffith provides UMD (Universal Module Definition) patterns for JavaScript modules that work everywhere.

## event system

Griffith defines a event system.

For the first frame duration, buffer count and other indicators commonly seen in video players, you can add a log by subscribing to the Griffith event.

For some features that need to communicate with the player, you can interact with the player by subscribing to the Griffith event.

## Streaming

Griffith uses Media Source Extensions™ to support streaming of mp4 and m3u8 formats.

- Preloading strategy: Griffith can dynamically control video loading progress through MSE.
- Automatically adjust video quality: Griffith can achieve Automatically adjusted video quality via MSE.

# Usage

## React application

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

## UMD
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

  // Create Player
  const player = Griffith.createPlayer(target)

  // Load video
  player.render({sources})

  // Dispose Player
  player.dispose()
</script>
```

All work on Griffith happens directly on GitHub. Both core team members and external contributors send pull requests which go through the same review process.

Feel free to fork and edit!
