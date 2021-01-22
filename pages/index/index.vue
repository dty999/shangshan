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
			<button @click="handleGuQu">古典纯音乐</button>
			<button @click="handleNvSheng">女声</button>
			
		</view>
	</view>
</template>

<script>
export default {

	data() {
		return {
			innerAudioContext: uni.createInnerAudioContext(),
			srcList: ['1436600583', '862114367', '28793052', '1431738249','1401456754','28302147','239026','412911546','409654891','306719','1340543218','1375935067','1347524822','1496851685','482169613','1421589049','5248591'],
			chunYinYueGuQu:['5267375','129490','128661','5267373'],
			nuSheng:['1436600583', '862114367', '28793052', '1431738249','1401456754','28302147','239026','412911546','409654891','306719','1340543218','1375935067','1347524822','1496851685','482169613','1421589049','5248591'],
			timer:null,
			playing:false,
			playImg:'../../static/music/bofang.png',
			bofangSrc:'../../static/music/bofang.png',
			zantingSrc:'../../static/music/zanting.png'
		};
	},
	computed:{
	},
	created() {
		console.log("页面组件创建");
		this.innerAudioContext.autoplay = false;
	},
	onLoad() {

	},
	methods: {
		handleGuQu(){
			 this.srcList = this.chunYinYueGuQu
			 // this.play()
		},
		handleNvSheng(){
			this.srcList = this.nuSheng
			// this.play()
		},
		handlePlayControl(){
			if(this.playing === false){
				this.play()
				this.playImg = this.zantingSrc
				this.playing = true
			}else if(this.playing === true){
				this.distory()
				this.playImg = this.bofangSrc
				this.playing = false
			}
		},
		play() {
			if(!this.innerAudioContext.paused){
				console.log("当前正在播放先暂停");
				this.innerAudioContext.stop()
			}
			this.playOne()
			this.innerAudioContext.onEnded(res => {
				console.log("播放下一首");
				this.playOne()
			});
			if(this.timer) clearTimeout(this.timer)
			this.timer = setTimeout(()=>{
				this.innerAudioContext.stop()
				console.log("时间到了,停止播放");
				this.playing = false
				this.playImg = this.bofangSrc
			},1000*60*10)
		},
		getRandom(m) {
			// 获取一个大于等于零小于m的数
			let num = null;
			num = parseInt(Math.random() * m);
			return num
		},
		distory() {
			if(this.timer){
				clearTimeout(this.timer)
			}
			this.innerAudioContext.stop()
			this.playing = false
			this.playImg = this.bofangSrc
			console.log("已销毁");
		},
		playOne(){
			console.log('play...');
			const index = this.getRandom(this.srcList.length);
			console.log(index);
			this.innerAudioContext.src = `http://link.hhtjim.com/163/${this.srcList[index]}.mp3`;
			console.log(this.innerAudioContext.src);
			this.innerAudioContext.play();
		}
	}
};
</script>

<style scoped>
.card_content .img{
	width: 70px;
	height: 70px;
	border-radius: 50%;
}
.playControl{
	width: 33px;
	height: 33px;
}
.card_content{
	font-size: 23px;
	display: flex;
	align-items: center;
	justify-content: space-between;
}


</style>
