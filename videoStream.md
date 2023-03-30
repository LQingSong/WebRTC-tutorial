# 4. 获取摄像头数据

## 你将会学到

- 获取设备的摄像头得到 video 视频流
- 操作视频流与回放
- 用 CSS 和 SVG 去操作视频

## Coding

完整代码看项目 step-01

这里只看 js 文件

```js
// 使用严格模式，避免编程陷阱
"use strict";

// 限制只获取video
const mediaStreamConstraints = {
  video: true,
};

// 获取video元素，用来播放localStream
const localVideo = document.querySelector("video");

// Local stream that will be reproduced on the video.
// localStream保存本地视频流
let localStream;

// 成功获取视频流时，将stream喂给video进行播放
// 使用的是 video.srcObject 属性
function gotLocalMediaStream(mediaStream) {
  localStream = mediaStream;
  localVideo.srcObject = mediaStream;
}

// Handles error by logging a message to the console with the error message.
function handleLocalMediaStreamError(error) {
  console.log("navigator.getUserMedia error: ", error);
}

// 初始化视频流
// getUserMedia 获取本地流媒体设备
navigator.mediaDevices
  .getUserMedia(mediaStreamConstraints)
  .then(gotLocalMediaStream)
  .catch(handleLocalMediaStreamError);
```

## 实现原理

通过调用 `getUserMedia(constraints)` 方法，获取用户设备权限，成功后会返回一个 stream 媒体流，可以当作流媒体元素被用在 video 的 srcObject 属性上。

`constraints` 参数你还可以设置为如下配置：

```js
const constraints = {
  video: {
    width: {
      min: 1280,
    },
    height: {
      min: 720,
    },
  },
};
```

如果所设置的摄像机分辨率当前浏览器不支持，getUserMedia()的请求会被拒绝，通过一个。

列举几种场景的分辨率

```js
const qvgaConstraints = {
  video: { width: { exact: 320 }, height: { exact: 240 } },
};

const vgaConstraints = {
  video: { width: { exact: 640 }, height: { exact: 480 } },
};

const hdConstraints = {
  video: { width: { exact: 1280 }, height: { exact: 720 } },
};

const fullHdConstraints = {
  video: { width: { exact: 1920 }, height: { exact: 1080 } },
};

const televisionFourKConstraints = {
  video: { width: { exact: 3840 }, height: { exact: 2160 } },
};

const cinemaFourKConstraints = {
  video: { width: { exact: 4096 }, height: { exact: 2160 } },
};

const eightKConstraints = {
  video: { width: { exact: 7680 }, height: { exact: 4320 } },
};
```

width/height 可以设置的属性

- min：最小
- exact：精密
- max： 最大

下面列举了所有可能支持的约束，但并不是所有设备（浏览器）都支持。

```js
dictionary MediaTrackConstraintSet {
  ConstrainULong width;
  ConstrainULong height;
  ConstrainDouble aspectRatio;
  ConstrainDouble frameRate;
  ConstrainDOMString facingMode;
  ConstrainDOMString resizeMode;
  ConstrainULong sampleRate;
  ConstrainULong sampleSize;
  ConstrainBoolean echoCancellation;
  ConstrainBoolean autoGainControl;
  ConstrainBoolean noiseSuppression;
  ConstrainDouble latency;
  ConstrainULong channelCount;
  ConstrainDOMString deviceId;
  ConstrainDOMString groupId;
};
```

## 加分项

- 因为 localStream 是在全局作用域下，所以在 Chrome 按 ctrl+shift+J 打开控制台，键入 localStream 可以查看视频流对象
- `localStream.getVideoTracks()` 会返回什么？
  <img alt="picture 1" src="images/%E8%8E%B7%E5%8F%96%E6%91%84%E5%83%8F%E5%A4%B4%E6%95%B0%E6%8D%AE/IMG_20230328-213740414.png" />

- 试试调用`localStream.getVideoTracks()[0].stop()`，看看会发生什么？

- 尝试修改`constraints`对象为`{ audio: true, video: true }`，看看会发生什么？

- video 标签元素的尺寸是多少？怎么通过 Javascript 获取 video 视频原始的尺寸大小，而不是显示的大小？使用 Chrome dev tools 去验证。
  <img alt="picture 2" src="images/%E8%8E%B7%E5%8F%96%E6%91%84%E5%83%8F%E5%A4%B4%E6%95%B0%E6%8D%AE/IMG_20230328-214919751.png" />
  可以通过 dev tools 的 Layers 去验证
  <img alt="picture 3" src="images/%E8%8E%B7%E5%8F%96%E6%91%84%E5%83%8F%E5%A4%B4%E6%95%B0%E6%8D%AE/IMG_20230328-215002218.png" />

- 尝试给 Video 标签元素添加 CSS filters。比如：

  ```css
  video {
    filter: blur(4px) invert(1) opacity(0.5);
  }
  ```

  或者添加一个 SVG 滤镜：

  ```css
  video {
    filter: hue-rotate(180deg) saturate(200%);
  }
  ```

## 你已经学会了

- [x] 能获取摄像头数据并显示在 video 标签上
- [x] 设置 media 的约束 constraints
- [x] 给 video 添加滤镜

## Tips

- 不要忘记给 video 标签添加 autoplay 属性，否则你只能看到一帧图像。
- 你可以通过 https://webrtc.github.io/samples/src/content/peerconnection/constraints/ 这个地址去尝试更多关于`getUserMedia`的 `constraints` 约束条件。

## 最佳实践

- 始终确保 video 标签有一个确定的尺寸大小不要溢出容器大小。通过添加`width`和`max-width` css 属性来设置 video 的首先大小和最大大小，高度通过浏览器自动计算得出：

  ```css
  video {
    max-width: 100%;
    width: 320px;
  }
  ```
