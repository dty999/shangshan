<template>
	<view class="content">
		<label>
			请输入时间
			<input type="number" placeholder="加油" v-model="fenzhong" />
		</label>
		<button @click="start">开始</button>
		<button @click="stop">结束</button>
		<view class="text">已坚持{{ fenzhong - showTime }}分钟</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			fenzhong: null,
			showTime:null,
			
			innerAudioContext: uni.createInnerAudioContext(),
			playTimer:null,
			Timer:null
		};
	},
	computed:{
		showTime2(){
			let num = this.fenzhong - this.showTime
			if( typeof(num) !== Number){
				// this.showTime2 = 0
				return 0
			}
			return num
		}
	},
	created() {
		console.log("页面组件创建");
		this.innerAudioContext.autoplay = false;
		this.innerAudioContext.src='../../static/home/zhongsheng.wav'
	},
	methods: {
		start() {
			console.log(this.fenzhong);
			if(this.fenzhong > 30){
				return 
			}
			this.showTime = this.fenzhong
			if(this.playTimer!==null)
				clearInterval(this.playTimer)
			this.playTimer=setInterval(()=>{
				this.showTime = this.showTime -1
				// if(this.showTime == 0){
				// 	this.innerAudioContext.src = ''
				// 	this.innerAudioContext.play()
				// 	return
				// }
				// this.innerAudioContext.play()
			},1000*60)
			if(this.Timer !== null)
				clearTimeout(this.Timer)
			this.Timer = setTimeout(()=>{
				this.innerAudioContext.play()
				this.stop()
			},this.fenzhong*1000*60)
		},
		stop(){
			clearInterval(this.playTimer)
			clearTimeout(this.Timer)
			this.fenzhong = null
			this.showTime = null
			console.log("清除定时器");
		}
	}
};
</script>

<style scoped>
label {
	display: flex;
	font-size: 16px;
	margin: 10px 10px;
	justify-content: space-between;
	align-items: center;
	border-bottom: 1px solid #eeeeee;
}
.text {
	font-size: 40px;
	text-align: center;
	line-height: 100px;
}
</style>
