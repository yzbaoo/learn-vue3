<template>
  <div>
    <h1>1 v 1</h1>
    <div class="video-container">
      <div class="video-player">
        <video ref="localPlayer" autoplay></video>
      </div>
      <div class="video-player">
        <video ref="remotePlayer" autoplay></video>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VideoStreamView',
  data() {
    return {
      peerConnection: null,
    }
  },
  mounted() {
   this.startVideoCall()
  },
  methods: {
    async getLocalStream () {
      const stream = await navigator.mediaDevices.getUserMedia({ video: true })
      this.$refs.localPlayer.srcObject = stream
      // this.$refs.remotePlayer.srcObject = stream
      return stream
    },  
    // 创建对等链接
    async createPeerConnection() {
      if(this.peerConnection) return this.peerConnection

      const configuration = {
        iceServers: [
          {urls: 'stun:stun.l.google.com:19302'}, 
          {urls: 'turn:turnserver.com', username: 'user', credential: 'pass'}
        ]
      }
      const peerConnection = new RTCPeerConnection(configuration);

      const localStream = await this.getLocalStream()
      localStream.getTracks().forEach(track => {
        peerConnection.addTrack(track, localStream)
      })

      // 处理远程媒体流
      peerConnection.ontrack = event => {
        const remoteStream = event.streams[0];
        this.$refs.remotePlayer.srcObject = remoteStream
      }
      // 处理ICE候选
      peerConnection.onicecandidate = event => {
        if (event.candidate) {
          // 1.将 ICE 候选发送给 对方
          // 示例：ws.send('sendICECandidate', event.candidate)
          // 2.同时监听对方 ICE 候选
          // 示例：ws.on('receiveICECandidate', (candidate) => this.receiveICECandidate(candidate))
        }
      }
      this.peerConnection = peerConnection

      return peerConnection
    },
    async startVideoCall() {
      const peerConnection = await this.createPeerConnection()

      const shareCode = this.$route.query.shareCode; // 获取 URL 参数中的 shareCode
      if(shareCode) {
        // 如果是被邀请者
        // 1. 监听ws 接收SDP offer
        // 示例：ws.on('receiveSDPOffer', (sdpOffer) => this.receiveSDPOffer(sdpOffer))
        // 2. 创建 answer
        const answer = await peerConnection.createAnswer()
        await peerConnection.setLocalDescription(answer)
        // 3. 发送 answer
        // 示例：ws.send('sendSDPAnswer', answer)
        
      }else {
        // 如果是邀请者
        // 1. 监听ws 接收SDP answer
        // 示例：ws.on('receiveSDPAnswer', (sdpAnswer) => this.receiveSDPAnswer(sdpAnswer))
        // 2. 创建 offer
        const offer = await peerConnection.createOffer()
        await peerConnection.setLocalDescription(offer)
        // 3. 发送 offer
        // 示例：ws.send('sendSDPOffer', offer)
      }
    },
    async receiveSDPAnswer(sdpAnswer) {
      await peerConnection.setRemoteDescription(sdpAnswer)
    },
    async receiveSDPOffer(sdpOffer) {
      await peerConnection.setRemoteDescription(sdpOffer)
    },
    async receiveICECandidate(candidate) {
      await peerConnection.addIceCandidate(new RTCIceCandidate(candidate))
    }
  }

};
  </script>

  <style>
  .video-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
  }
  .video-player {
    flex: 1;
    object-fit: cover;
  }
  </style>
