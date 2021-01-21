<template>
	<view>
		<!-- 		<uni-card :title="remember.itemName" :thumbnail="remember.image" :extra="remember.extra" :note="remember.note">
			<view class="card_content">
				<text space="nbsp">{{ remember.msg }}</text>
				<view>
					<image :src="remember.failImg" class="logoBtn"></image>
					<image :src="remember.setImg" class="logoBtn"></image>
				</view>
			</view>
		</uni-card> -->
		<uni-card v-for="(item, index) in rememberList" :title="item.itemName" :extra="item.extra === '' ? '正计时' : `倒计时${item.extra}天`" :note="item.note">
			<view class="card_content">
				<text space="nbsp">{{ item | TianShu }}</text>
				<view>
					<image :src="successImg" class="logoBtn"></image>
					<image :src="failImg" class="logoBtn" @click="fail(index)"></image>
				</view>
			</view>
		</uni-card>
		<image src="../../static/remember/add.png" class="add" @click="add"></image>
		<text>打卡</text>
	</view>
</template>

<script>
import Card from '../../components/uni-card/uni-card.vue';
import uniPopup from '@/components/uni-popup/uni-popup.vue';
import uniPopupMessage from '@/components/uni-popup/uni-popup-message.vue';
import uniPopupDialog from '@/components/uni-popup/uni-popup-dialog.vue';
export default {
	components: {
		Card,
		uniPopup,
		uniPopupMessage,
		uniPopupDialog
	},
	data() {
		return {
			rememberList: null,
			failImg: '../../static/remember/不开心.png',
			successImg: '../../static/remember/开心.png',
			remember: {
				itemName: '读书',
				msg: '5 天',
				startTime: new Date(),
				image: '../../static/remember/读书.png',
				failImg: '../../static/remember/失败.png',
				setImg: '../../static/remember/设置.png',
				extra: '正计时',
				note: '阅读是和古人对话'
			}
		};
	},
	filters: {
		TianShu: function(item) {
			const newDate = new Date();
			const time = newDate.getTime()-item.thisTime
			let day = 0
			if(time<1000){
				day = 0
			}else{
				day = parseInt(time / (1000 * 60 * 60 * 24));
			}
			if (item.extra === '') {
				return `已坚持${day}天`;
			} else {
				return `还剩${item.extra-day}天`;
			}
		}
	},
	created() {
		this.getRememberList();
	},
	onShow: function() {
		this.getRememberList();
	},
	methods: {
		add() {
			// 路径不用谢pages
			uni.navigateTo({
				url: `../Remember/AddItem/AddItem`
			});
		},
		getRememberList() {
			try {
				const value = uni.getStorageSync('rememberList');
				if (value) {
					// console.log(JSON.parse(value));
					this.rememberList = JSON.parse(value);
				}
			} catch (e) {
				console.log('获取存储失败');
			}
		},
		fail(index) {
			// console.log(index);
			const curTime = new Date()
			const item = this.rememberList[index]
			item.thisTime = curTime.getTime()
			this.rememberList[index].arr.push(item.thisTime);
			// console.log(this.rememberList[index]);
			uni.setStorage({
				key: 'rememberList',
				data: JSON.stringify(this.rememberList)
			});
		}
	}
};
</script>

<style>
.card_content {
	display: flex;
	justify-content: space-between;
	font-size: 27px;
	align-items: center;
	/* text-align: center; */
	color: #09bb07;
}
.card_content text {
	flex-grow: 1;
}
.logoBtn {
	width: 23px;
	height: 23px;
	margin-left: 15px;
}
.add {
	width: 46px;
	height: 46px;
	position: fixed;
	bottom: 70px;
	right: 15px;
}
</style>
