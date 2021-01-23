<template>
	<view>
		<uni-card v-for="(item, index) in rememberList" :title="item.itemName" :extra="item.extra === '' ? '正计时' : `倒计时${item.extra}天`" :note="item.note">
			<view class="card_content">
				<text space="nbsp">{{ item | TianShu }}</text>
				<view>
					<image :src="'../../static/remember/kaixin.png'" class="logoBtn"></image>
					<image :src="'../../static/remember/bukaixin.png'" class="logoBtn" @click="fail(index)"></image>
					<image :src="'../../static/remember/shanchu.png'" class="logoBtn" @click="deleteItem(index)"></image>
				</view>
			</view>
		</uni-card>
		<image :src="'../../static/remember/add.png'" class="add" @click="add"></image>
		<uni-popup ref="popup" type="center"><uni-popup-message type="success" :message="popupMsg" :duration="2000"></uni-popup-message></uni-popup>
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
			failImg: '../../static/remember/bukaixin.png',
			successImg: '../../static/remember/kaixin.png',
			remember: {
				itemName: '读书',
				msg: '5 天',
				startTime: new Date(),
				image: '../../static/remember/dushu.png',
				failImg: '../../static/remember/bukaixin.png',
				setImg: '../../static/remember/shezhi.png',
				extra: '正计时',
				note: '阅读是和古人对话'
			},
			popupMsg: '保存成功'
		};
	},
	filters: {
		TianShu: function(item) {
			const newDate = new Date();
			const time = newDate.getTime() - item.thisTime;
			let day = 0;
			let min = 0;
			let hour = 0;
			if (time < 1000) {
				day = 0;
			} else {
				day = parseInt(time / (1000 * 60 * 60 * 24));
				hour = parseInt(time % (1000 * 60 * 60 * 24)/(1000 * 60 * 60))
				min = parseInt((time % (1000 * 60 * 60 * 24)%(1000 * 60 * 60))/(1000*60));
			}
			if (item.extra === '') {
				return `已坚持${day}天${hour}时${min}分`;
			} else {
				if (item.extra - day <= 0) {
					return `恭喜你!已经完成任务!`;
				}
				return `还剩${item.extra - day}天`;
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
		deleteItem(index) {
			this.rememberList.splice(index, 1);
			this.saveRememberList('删除成功');
		},
		add() {
			// 路径不用谢pages
			console.log('添加按钮按下');
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
			const curTime = new Date();
			const item = this.rememberList[index];
			item.thisTime = curTime.getTime();
			this.rememberList[index].arr.push(item.thisTime);
			this.saveRememberList('继续努力');
		},
		saveRememberList(msg) {
			uni.setStorage({
				key: 'rememberList',
				data: JSON.stringify(this.rememberList),
				success: () => {
					console.log('success');
					this.popupMsg = msg;
					this.$refs.popup.open();
				}
			});
		}
	}
};
</script>

<style>
.card_content {
	display: flex;
	justify-content: space-between;
	font-size: 19px;
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
