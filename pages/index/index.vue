<template>
	<view class="content">
		<button @click="play">播放</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		onLoad() {
			this.show = true
		},
		methods: {
			play(){
				const innerAudioContext = uni.createInnerAudioContext();
				innerAudioContext.autoplay = true;
				innerAudioContext.src = 'http://link.hhtjim.com/163/27678655.mp3';
				innerAudioContext.onPlay(() => {
				  console.log('开始播放');
				});
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
