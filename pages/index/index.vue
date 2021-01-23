<template>
	<view class="content">
		<uni-card class="card">
			<view class="card_content">
				<image :src="'../../static/music/hehua.jpg'" class="img"></image>
				一夜无梦
				<image :src="playImg" class="playControl" @click="handlePlayControl"></image>
			</view>
		</uni-card>
		<view class="cate">
			<!-- <button @click="handleGuQu">古典纯音乐</button> -->
			<!-- <button @click="handleNvSheng">女声</button> -->
			<button @click="handleTiGang">提肛运动{{isTiGang?'正在运动':''}}</button>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			isTiGang: false,
			tiGangContext: uni.createInnerAudioContext(),
			innerAudioContext: uni.createInnerAudioContext(),
			srcList: ['../../static/music/gaoshanliushui.mp3', '../../static/music/liangxiaoyin.mp3', '../../static/music/wangningmei.mp3'],

			timer: null,
			playing: false,
			playImg: '../../static/music/bofang.png',
			bofangSrc: '../../static/music/bofang.png',
			zantingSrc: '../../static/music/zanting.png'
		};
	},
	computed: {},
	created() {
		console.log('页面组件创建');
		this.innerAudioContext.autoplay = false;
		this.tiGangContext.autoplay = false;
		this.tiGangContext.src = '../../static/music/tigang.m4a';
	},
	onLoad() {},
	methods: {
		handleTiGang() {
			this.distory()
			if (this.isTiGang === false) {
				this.isTiGang = true
				this.tiGangContext.play();
			}else{
				this.isTiGang = false
				this.tiGangContext.stop()
			}
		},
		handlePlayControl() {
			if (this.playing === false) {
				this.play();
				this.playImg = this.zantingSrc;
				this.playing = true;
			} else if (this.playing === true) {
				this.distory();
				this.playImg = this.bofangSrc;
				this.playing = false;
			}
		},
		play() {
			if (!this.innerAudioContext.paused) {
				console.log('当前正在播放先暂停');
				this.innerAudioContext.stop();
			}
			this.playOne();
			this.innerAudioContext.onEnded(res => {
				console.log('播放下一首');
				this.playOne();
			});
			if (this.timer) clearTimeout(this.timer);
			this.timer = setTimeout(() => {
				this.innerAudioContext.stop();
				console.log('时间到了,停止播放');
				this.playing = false;
				this.playImg = this.bofangSrc;
			}, 1000 * 60 * 10);
		},
		getRandom(m) {
			// 获取一个大于等于零小于m的数
			let num = null;
			num = parseInt(Math.random() * m);
			return num;
		},
		distory() {
			if (this.timer) {
				clearTimeout(this.timer);
			}
			this.innerAudioContext.stop();
			this.playing = false;
			this.playImg = this.bofangSrc;
			console.log('已销毁');
		},
		playOne() {
			console.log('play...');
			const index = this.getRandom(this.srcList.length);
			console.log(index);
			this.innerAudioContext.src = this.srcList[index];
			console.log(this.innerAudioContext.src);
			this.innerAudioContext.play();
		}
	}
};
</script>

<style scoped>
.card_content .img {
	width: 70px;
	height: 70px;
	border-radius: 50%;
}
.playControl {
	width: 33px;
	height: 33px;
}
.card_content {
	font-size: 23px;
	display: flex;
	align-items: center;
	justify-content: space-between;
}
</style>
