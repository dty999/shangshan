<template>
	<view>
		<label>
			事项名称
			<input type="text" placeholder="请输入事项名称" v-model="remember.itemName" />
		</label>
		<label>
			一句话
			<input type="text" placeholder="请输入一句话" v-model="remember.note" />
		</label>
		<label>
			倒计时
			<input type="text" placeholder="输入天数,默认是正计时" v-model="remember.extra" />
		</label>
		<button type="default" @click="submit">提交</button>
	</view>
</template>

<script>
export default {
	onShow: function(option) {
		try {
			const value = uni.getStorageSync('rememberList');
			if (value) {
				console.log(JSON.parse(value))
				this.rememberList = JSON.parse(value)
			}
		} catch (e) {
			console.log('获取存储失败');
		}
	},
	data() {
		return {
			rememberList : [],
			remember: {
				itemName: '',
				msg: '0',
				image: '../../static/remember/道教.png',
				extra: '',
				note: '',
				arr: [],
				exTime:'',
				thisTime:''
			}
		};
	},
	methods: {
		submit() {
			const curTime = new Date()
			this.remember.arr.push(curTime.getTime())
			this.remember.thisTime = curTime.getTime()
			this.rememberList.push(this.remember)
			uni.setStorage({
			    key: 'rememberList',
			    data: JSON.stringify(this.rememberList),
			})
			uni.navigateBack({
			    delta: 1
			});
		}
	}
};
</script>

<style lang="scss">
label {
	display: flex;
	font-size: 16px;
	margin: 10px 10px;
	justify-content: space-between;
	align-items: center;
	border-bottom: 1px solid #eeeeee;
}
label input {
	// border: 1px solid #EEEEEE;
}
</style>
