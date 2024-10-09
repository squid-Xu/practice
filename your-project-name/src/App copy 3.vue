<template>
	<div>
		<h1>测试语音播放</h1>
		<audio :loop="true" id="mp3Btn" ref="audioTip" style="visibility: hidden">
			<source :src="audioSrc" />
		</audio>
	</div>
</template>

<script>
export default {
	name: 'App',
	data() {
		return {
			audioSrc: '',
		};
	},
	mounted() {
		this.audioSrc = require('./assets/red.mp3');
		if (typeof window.WeixinJSBridge === 'undefined') {
			//未执行WeixinJSBridge 开始监听 WeixinJSBridge
			document.addEventListener(
				'WeixinJSBridgeReady',
				() => {
					document.getElementById('mp3Btn').play();
				},
				false
			);
		} else {
			//已经执行 使用 getNetworkType 调用获取网络类型后执行
			window.WeixinJSBridge.invoke('getNetworkType', {}, () => {
				document.getElementById('mp3Btn').play();
			});
		}
	},
};
</script>
