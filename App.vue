<script>
	export default {
		onLaunch: function(option) {
			console.log('App Launch')
			// console.log(option);
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style>
	/*每个页面公共css */
	@import "/common/uni.css";
	@import "/common/common.css";
	@import "/common/icon.css";
	@import "/common/animate.css";
</style>
