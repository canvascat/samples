# WebRTC Code Samples

This is a repository for the WebRTC JavaScript code samples. All of the samples can be tested from [webrtc.github.io/samples](https://webrtc.github.io/samples).

We welcome contributions and bugfixes. Please see [CONTRIBUTING.md](https://github.com/webrtc/samples/blob/gh-pages/CONTRIBUTING.md) for details.

## API

### Devices

https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/setSinkId
`HTMLMediaElement.setSinkId(sinkId)` 设置用于输出的音频设备的 ID

https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getUserMedia
设置用于输入的音视频设备的 ID 后重新获取 `MediaStream` 赋给媒体 `HTMLMediaElement`
`navigator.mediaDevices.getUserMedia(MediaStreamConstraints)`
通过设置 `MediaStreamConstraints` 返回合乎条件的媒体流，设置 `deviceId`/`width`/`height`等，或设置`video`/`audio`为`false`来禁止输入
设置`width`/`height`来返回指定分辨率的媒体流，支持高分辨率适应低分辨率，反之抛出`OverconstrainedError`
https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack/applyConstraints
通过 `MediaStreamTrack.applyConstraints()` 来约束媒体流分辨率宽高横纵比等
https://developer.mozilla.org/en-US/docs/Web/API/MediaDevices/getDisplayMedia
通过 `navigator.mediaDevices.getDisplayMedia()` 获取窗口内容的媒体流

通过 `HTMLCanvasElement` 抓取视频快照
`HTMLCanvasElement.getContext('2d').drawImage(HTMLVideoElement, 0, 0, HTMLCanvasElement.width, HTMLCanvasElement.height)`
https://developer.mozilla.org/en-US/docs/Web/CSS/filter
通过 `filter` 样式给 `HTMLVideoElement` 添加滤镜可以抓取带滤镜效果的视频快照

https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder
通过 `MediaRecorder` 实现对 `MediaStream` 的录制，通过 `Blob` 对数据进行处理
```js
const recordedBlobs = [];
const mimeType = 'video/mp4;codecs=h264,aac';
const mediaRecorder = new MediaRecorder(window.stream, { mimeType });
mediaRecorder.onstop = (event) => {
  const blob = new Blob(recordedBlobs, { type: 'video/webm' });
  const url = window.URL.createObjectURL(blob);
  // Do something
};
mediaRecorder.ondataavailable = (e) => {
  e.data && e.data.size > 0 && recordedBlobs.push(e.data);
};
mediaRecorder.start();
```

https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamTrack
对媒体轨道进行处理，获取每个可调节属性的值或者范围 `MediaStreamTrack.getCapabilities()`，并使用 `MediaStreamTrack.applyConstraints()` 调节。
例子：https://webrtc.github.io/samples/src/content/getusermedia/pan-tilt-zoom/

`HTMLMediaElement.captureStream()` 获取媒体元素的媒体流对象，可以作用于`HTMLCanvasElement`
通过将原视频绘制到canvas上再进行处理后`putImageData`到用于输出的canvas上，可以对流媒体内容实时处理，通常配合`requestAnimationFrame`/`Worker`使用
例子：https://webrtc.github.io/samples/src/content/capture/canvas-filter/ & https://webrtc.github.io/samples/src/content/capture/worker-process

## RTCDataChannel
> https://developer.mozilla.org/zh-CN/docs/Web/API/RTCDataChannel
本地PC通过`RTCDataChannel.createDataChannel()`建立DC，远程PC通过`RTCPeerConnection.ondatachannel`监听事件使用`RTCDataChannelEvent.channel`获取DC。
通过设置`RTCDataChannel.binaryType`来传输文件
例子：https://webrtc.github.io/samples/src/content/datachannel/messaging/，顺便复习下 `customElements`
