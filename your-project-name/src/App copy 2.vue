<template>
	<div>测试语音播放</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {
			context: null,
			source: null,
			audioBuffer: null,
		};
	},
	mounted() {
		this.start();
	},
	methods: {
		start() {
			window.AudioContext =
				window.AudioContext || window.webkitAudioContext || window.mozAudioContext || window.msAudioContext;
			this.context = new window.AudioContext();
			this.audioBuffer = null;
			this.loadAudioFile(require('./assets/red.mp3'));
		},
		// 加载音频文件
		loadAudioFile(url) {
			var xhr = new XMLHttpRequest(); //通过XHR下载音频文件
			xhr.open('GET', url, true);
			xhr.responseType = 'arraybuffer';
			xhr.onload = () => {
				//下载完成
				this.initSound(xhr.response);
			};
			xhr.send();
		},
		// 解码音频文件
		initSound(arrayBuffer) {
			this.context.decodeAudioData(
				arrayBuffer,
				buffer => {
					//解码成功时的回调函数
					this.audioBuffer = buffer;
					this.playSound();
				},
				e => {
					//解码出错时的回调函数
					console.log('Error decoding file', e);
				}
			);
		},
		// 播放音频
		playSound() {
			this.source = this.context.createBufferSource(); //获取音频数据
			this.source.buffer = this.audioBuffer;
			this.source.loop = true;
			this.source.connect(this.context.destination);
			this.source.start(); //立即播放
		},
	},
};
</script>
