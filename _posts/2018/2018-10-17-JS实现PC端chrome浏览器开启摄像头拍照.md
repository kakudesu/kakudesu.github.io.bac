---
layout: post
title: JS实现PC端chrome浏览器开启摄像头拍照
author: suki
date: 2018-10-17
tags:
- Chrome
- 摄像头
english: false
---

## JS实现PC端chrome浏览器开启摄像头拍照

### 代码:

```

	<div id="screenshot" style="text-align:center;border: 1px dashed #000">
        <video width="500" height="500" class="videostream" autoplay=""></video>
        <img id="screenshot-img">
        <p>
            <button class="capture-button">Capture video</button>
        </p>
        <p>
            <button id="screenshot-button" disabled="">Take screenshot</button>
        </p>
    </div>

    <script>
        
        const constraints = {
            video: true
        };
        const captureVideoButton =
            document.querySelector('#screenshot .capture-button');
        const screenshotButton = document.querySelector('#screenshot-button');
        const img = document.querySelector('#screenshot img');
        const video = document.querySelector('#screenshot video');

        const canvas = document.createElement('canvas');

        captureVideoButton.onclick = function () {
            navigator.mediaDevices.getUserMedia(constraints).
                then(handleSuccess).catch(handleError);
        };

        screenshotButton.onclick = video.onclick = function () {
            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            canvas.getContext('2d').drawImage(video, 0, 0);
            // Other browsers will fall back to image/png
            img.src = canvas.toDataURL('image/webp');
            console.log(img.src);
        };

        function handleSuccess(stream) {
            screenshotButton.disabled = false;
            video.srcObject = stream;
        }
        function handleError(error) {
            console.error('CapturingVideoScreenShotError: ', error);
        }
    </script>
    
```

### 原文链接:

> https://www.html5rocks.com/en/tutorials/getusermedia/intro/

### 参考资料:

> https://webrtc.github.io/samples/src/content/getusermedia/resolution/

### WebRTC github仓库:

> https://github.com/webrtc/samples/blob/gh-pages/src/content/getusermedia/resolution