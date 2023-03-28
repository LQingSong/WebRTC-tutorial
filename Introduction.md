# 1. 介绍

根据官网教学
https://codelabs.developers.google.com/codelabs/webrtc-web

翻译学习

## WebRTC 是一个开源项目，可以用于 Web 和原生 App 的实时音视频数据传输。

## webRTC 有一些 JavaScriptAPI：

- `getUserMedia`: 捕获摄像头和音频数据
- `MediaRecorder`： 录音和录屏
- `RTCPeerConnection`: 在用户之间的传输音频流和视频流
- `RTCDataChannel`: 用户之间的视频流

## 可以在哪里使用

火狐、Opera 和 谷歌浏览器，也可以用于原生安卓和 IOS。

## 什么是 Signaling ？

WebRTC 使用 RTCPeerConnection 在浏览器（用户）之间传输数据（音视频数据流），需要通过一种机制来协调通信和传输消息，这种机制被称作 Signaling 信令协议。WebRTC 对信令服务没有做限制，允许使用任何一种方案当作信令服务进行消息的传输。比如 HTTP/Socket、SIP 等等。

## 什么是 STUN 和 TURN ？

在真实网络环境下，两个用户可能由于 NAT 和防火墙的策略导致并不能直连，webRTC 就需要通过 STUN 服务去获取公网 IP 地址，还有一些情况还需要通过 TURN 服务作为替代方案用于连接。

## WebRTC 安全吗？

加密是所有 WebRTC 组件的强制要求，并且 WebRTC JavascriptAPI 只允许使用在安全源下。

由于 Signaling 没有做标准限制，所以这取决于你使用安全的协议。
