# 빠르게 배우는 WebRTC

[기초지식](Basic.md)을 토대로 최대한 빠르게 간단한 WebRTC 애플리케이션을 구현해보겠습니다.

통신에 사용할 웹캠 혹은 마이크를 준비해주세요.

웹캠이나 마이크가 없으신가요? 대체할 방법이 있으니 걱정없이 따라오시면 됩니다 :)

## 준비하기

---

의존 패키지 설치

```
npm install express, socket.io
```

```javascript
const express = require("express");
```

## 미디어 스트림 얻기

---

페이크 데이터

```javascript
const localVideo = document.getElementById("localVideo");
let localStream;

const mediaConstraints = {
  video: true,
  audio: true
};

async function getMyStream() {
  try {
    localStream = await navigator.mediaDevices.getUserMedia(mediaConstraints);
    localVideo.srcObject = localStram;
  } catch (err) {
    console.error(err);
  }
}
```

## 호 처리하기(Signaling)

---

### 서버

```javascript
socket.on("sendOffer", offerSDP => {
  socket.broadcast.emit("receiveOffer", offerSDP);
});

socket.on("sendAnswer", answerSDP => {
  socket.broadcast.emit("receiveAnswer", answerSDP);
});
```

### 클라이언트

```javascript
const socket = io();

socket.on("receiveOffer", offerSDP => {
  pc.setRemoteDescription(offerSDP);
});

socket.on("receiveAnswer", answerSDP => {
  pc.setRemoteDescription(answerSDP);
});
```

## 통신하기

---

### 서버

```javascript
socket.on("icecandidate", async candidate => {
  await pc.addIceCandidate(candidate);
});
```

### 클라이언트

```javascript
socket.on("icecandidate", async candidate => {
  await pc.addIceCandidate(candidate);
});
```

## DataChannel

---

데이터 채널
