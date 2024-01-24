---
title: WebRTC实现前端代码
date: 2023-02-24 09:46:09
author: 长白崎
categories:
  - "WebRTC"
tags:
  - "WebRTC"
  - "HTML"
  - "前端"
---



```html
<!DOCTYPE html>
<html>

<head>
    <title>RTC视频通话测试页面</title>
    <meta charset="UTF-8"> <!-- for HTML5 -->
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1" />
</head>
<body>
<style type="text/css">
    body {
        background: #000;
    }

    button {
        height: 40px;
        line-height: 40px;
        width: 500px;
        top: 100px;
        padding: 0 15px;
        contain: content;
        background: #ccc;
        border: none;
        border-radius: 10px;
        margin-bottom: 10px;
        overflow: hidden;
    }

    .wrap {
        width: 100vw;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .video-box {
        border-radius: 20px;
        background: pink;
        position: relative;
        width: 800px;
        height: 600px;
        overflow: hidden;
    }

    .remote-video {
        width: 800px;
        height: 600px;
        border: 1px solid black;
        overflow: hidden;
    }

    .local-video {
        width: 320px;
        height: 240px;
        position: absolute;
        right: 0;
        bottom: 0;
        border-radius: 20px 0 0 0;
        overflow: hidden;
    }

    video {
        width: 100%;
        height: 100%;
    }
</style>
<div class="wrap">
    <div>
        <div class="video-box">
            <div class="local-video">
                <video id="local-video" autoplay style=""></video>
            </div>
            <div class="remote-video">
                <video id="remote-video" autoplay></video>
            </div>
        </div>
        <div>
            <button type="button" onclick="startVideo();">开启本机摄像和音频</button>
            <button type="button" onclick="connect();">建立连接</button>
            <button type="button" onclick="hangUp();">挂断</button>
            <button type="button" onclick="refreshPage();">刷新页面</button>
        </div>
    </div>

</div>

<script>
    // ===================以下是socket=======================
    var user = Math.round(Math.random() * 1000) + ""
    //var socketUrl = "ws://localhost:8080/msgServer/" + user;
    var socketUrl = "ws://172.29.40.176:8080/msgServer/"+ user;
    var socket = null
    var socketRead = false
    window.onload = function() {

        socket = new WebSocket(socketUrl)
        socket.onopen = function() {
            console.log("成功连接到服务器...")
            socketRead = true
        }
        socket.onclose = function(e) {
            console.log('与服务器连接关闭: ' + e.code)
            socketRead = false
        }

        socket.onmessage = function(res) {
            var evt = JSON.parse(res.data)
            console.log(evt)
            if (evt.type === 'offer') {
                console.log("接收到offer,设置offer,发送answer....")
                onOffer(evt);
            } else if (evt.type === 'answer' && peerStarted) {
                console.log('接收到answer,设置answer SDP');
                onAnswer(evt);
            } else if (evt.type === 'candidate' && peerStarted) {
                console.log('接收到ICE候选者..');
                onCandidate(evt);
            } else if (evt.type === 'bye' && peerStarted) {
                console.log("WebRTC通信断开");
                stop();
            }
        }
    }

    // ===================以上是socket=======================

    var localVideo = document.getElementById('local-video');
    var remoteVideo = document.getElementById('remote-video');
    var localStream = null;
    var peerConnection = null;
    var peerStarted = false;
    var mediaConstraints = {
        'mandatory': {
            'OfferToReceiveAudio': false,
            'OfferToReceiveVideo': true
        }
    };

    //----------------------交换信息 -----------------------

    function onOffer(evt) {
        console.log("接收到offer...")
        console.log(evt);
        setOffer(evt);
        sendAnswer(evt);
        peerStarted = true
    }

    function onAnswer(evt) {
        console.log("接收到Answer...")
        console.log(evt);
        setAnswer(evt);
    }

    function onCandidate(evt) {
        var candidate = new RTCIceCandidate({
            sdpMLineIndex: evt.sdpMLineIndex,
            sdpMid: evt.sdpMid,
            candidate: evt.candidate
        });
        console.log("接收到Candidate...")
        console.log(candidate);
        peerConnection.addIceCandidate(candidate);
    }

    function sendSDP(sdp) {
        var text = JSON.stringify(sdp);
        console.log('发送sdp.....')
        console.log(text); // "type":"offer"....
        // textForSendSDP.value = text;
        // 通过socket发送sdp
        socket.send(text)
    }

    function sendCandidate(candidate) {
        var text = JSON.stringify(candidate);
        console.log(text); // "type":"candidate","sdpMLineIndex":0,"sdpMid":"0","candidate":"....
        socket.send(text) // socket发送
    }

    //---------------------- 视频处理 -----------------------
    function startVideo() {

        navigator.mediaDevices.enumerateDevices()
        .then(function (devices) {
            devices.forEach(function (device) {
                //打印设备类型，设备名称，设备ID
                console.log(device.kind+":"+device.label+" id="+device.deviceId);
                //根据设备名称，去获取特定的摄像头
                if(device.label.indexOf("Logitech")!=-1){

                }
            })
        });

        if (navigator.mediaDevices.getUserMedia) {
            //最新的标准API
            navigator.mediaDevices.getUserMedia({
                video: true,
                audio: true
            }).then(function(stream) { //success
                localStream = stream;
                localVideo.srcObject = stream;
                //localVideo.src = window.URL.createObjectURL(stream);
                localVideo.play();
                localVideo.volume = 0;
            }).catch(function(error) { //error
                console.error('发生了一个错误: [错误代码：' + error.code + ']');
                return;
            });
        } else if (navigator.webkitGetUserMedia) {
            //webkit核心浏览器
            navigator.webkitGetUserMedia({
                video: true,
                audio: true
            },function(stream) { //success
                localStream = stream;
                localVideo.srcObject = stream;
                //localVideo.src = window.URL.createObjectURL(stream);
                localVideo.play();
                localVideo.volume = 0;
            }, error)
        } else if (navigator.mozGetUserMedia) {
            //firfox浏览器
            navigator.mozGetUserMedia({
                video: true,
                audio: true
            }, function(stream) { //success
                localStream = stream;
                localVideo.srcObject = stream;
                //localVideo.src = window.URL.createObjectURL(stream);
                localVideo.play();
                localVideo.volume = 0;
            }, error);
        } else if (navigator.getUserMedia) {
            //旧版API
            navigator.getUserMedia({
                video: true,
                audio: true
            }, function(stream) { //success
                localStream = stream;
                localVideo.srcObject = stream;
                //localVideo.src = window.URL.createObjectURL(stream);
                localVideo.play();
                localVideo.volume = 0;
            }, error);
        }
        
    }

    function refreshPage() {
        location.reload();
    }

    //---------------------- 处理连接 -----------------------
    function prepareNewConnection() {
        var pc_config = {
            "iceServers": []
        };
        var peer = null;
        try {
            peer = new RTCPeerConnection(pc_config);
        } catch (e) {
            console.log("建立连接失败，错误：" + e.message);
        }

        // 发送所有ICE候选者给对方
        peer.onicecandidate = function(evt) {
            if (evt.candidate) {
                console.log(evt.candidate);
                sendCandidate({
                    type: "candidate",
                    sdpMLineIndex: evt.candidate.sdpMLineIndex,
                    sdpMid: evt.candidate.sdpMid,
                    candidate: evt.candidate.candidate
                });
            }
        };
        console.log('添加本地视频流...');
        peer.addStream(localStream);

        peer.addEventListener("addstream", onRemoteStreamAdded, false);
        peer.addEventListener("removestream", onRemoteStreamRemoved, false);

        // 当接收到远程视频流时，使用本地video元素进行显示
        function onRemoteStreamAdded(event) {
            console.log("添加远程视频流");
            // remoteVideo.src = window.URL.createObjectURL(event.stream);
            remoteVideo.srcObject = event.stream;
        }

        // 当远程结束通信时，取消本地video元素中的显示
        function onRemoteStreamRemoved(event) {
            console.log("移除远程视频流");
            remoteVideo.src = "";
        }

        return peer;
    }

    function sendOffer() {
        peerConnection = prepareNewConnection();
        peerConnection.createOffer(function(sessionDescription) { //成功时调用
            peerConnection.setLocalDescription(sessionDescription);
            console.log("发送: SDP");
            console.log(sessionDescription);
            sendSDP(sessionDescription);
        }, function(err) { //失败时调用
            console.log("创建Offer失败");
        }, mediaConstraints);
    }

    function setOffer(evt) {
        if (peerConnection) {
            console.error('peerConnection已存在!');
            return;
        }
        peerConnection = prepareNewConnection();
        peerConnection.setRemoteDescription(new RTCSessionDescription(evt));
    }

    function sendAnswer(evt) {
        console.log('发送Answer,创建远程会话描述...');
        if (!peerConnection) {
            console.error('peerConnection不存在!');
            return;
        }

        peerConnection.createAnswer(function(sessionDescription) { //成功时
            peerConnection.setLocalDescription(sessionDescription);
            console.log("发送: SDP");
            console.log(sessionDescription);
            sendSDP(sessionDescription);
        }, function() { //失败时
            console.log("创建Answer失败");
        }, mediaConstraints);
    }

    function setAnswer(evt) {
        if (!peerConnection) {
            console.error('peerConnection不存在!');
            return;
        }
        peerConnection.setRemoteDescription(new RTCSessionDescription(evt));
    }

    //-------- 处理用户UI事件 -----
    // 开始建立连接
    function connect() {
        if (!localStream) {
            window.alert("请首先捕获本地视频数据.");
        } else if (peerStarted || !socketRead) {
            window.alert("请刷新页面后重试.");
        } else {
            sendOffer();
            peerStarted = true;
        }
    }

    // 停止连接
    function hangUp() {
        console.log("挂断.");
        stop();
    }

    function stop() {
        peerConnection.close();
        peerConnection = null;
        peerStarted = false;
    }
</script>
</body>
</html>
```

