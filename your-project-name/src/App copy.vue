<template>
  <div>
    <audio src="./assets/red.mp3" id="myAudio" autoplay></audio>
  </div>
  <!-- <div>
    <h1>微信测试</h1>
    <audio :loop="true" id="mp3Btn" ref="audioTip" style="visibility: hidden">
      <source :src="audioSrc" />
    </audio>
  </div> -->
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      context: null,
      source: null,
      audioBuffer: null,
      audioSrc: ''
    };
  },
  mounted() {
    // this.audioSrc = require('./assets/red.mp3');
    // if (typeof window.WeixinJSBridge === "undefined") {
    //   //未执行WeixinJSBridge 开始监听 WeixinJSBridge
    //   document.addEventListener('WeixinJSBridgeReady', () => {
    //     document.getElementById('mp3Btn').play();
    //   }, false);
    // } else {
    //   //已经执行 使用 getNetworkType 调用获取网络类型后执行
    //   window.WeixinJSBridge.invoke("getNetworkType", {}, () => {
    //     document.getElementById('mp3Btn').play();
    //   });
    // }
    // this.start();
  },
  methods: {
    start() {
      window.AudioContext = window.AudioContext || window.webkitAudioContext || window.mozAudioContext || window.msAudioContext;
      this.context = new window.AudioContext();
      this.audioBuffer = null;

      this.loadAudioFile(require('./assets/red.mp3'))
    },
    loadAudioFile(url) {
      let _that = this
      var xhr = new XMLHttpRequest(); //通过XHR下载音频文件
      xhr.open('GET', url, true);
      xhr.responseType = 'arraybuffer';
      xhr.onload = function () { //下载完成
        console.log(this.response);

        _that.initSound(this.response);
      };
      xhr.send();
    },
    initSound(arrayBuffer) {
      let _this = this
      this.context.decodeAudioData(arrayBuffer, function (buffer) { //解码成功时的回调函数
        _this.audioBuffer = buffer;
        _this.playSound();
      }, function (e) { //解码出错时的回调函数
        console.log('Error decoding file', e);
      });
    },
    playSound() {
      this.source = this.context.createBufferSource();//获取音频数据
      this.source.buffer = this.audioBuffer;
      this.source.loop = true;
      this.source.connect(this.context.destination);
      this.source.start(); //立即播放
    }
  },
};
</script>
