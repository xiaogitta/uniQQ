<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
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
	@import "colorui/main.css";
    @import "colorui/icon.css";
	/*每个页面公共css */
	.box {
		margin: 20upx 0;
	}
	
	.box view.cu-bar {
		margin-top: 20upx;
	}
	.footer{
	  position: fixed;
	  bottom: 0;
	  width: 100%;
	  line-height: var(--footer-height);
	  color: #fff;
	}
	.main-overflow{
		overflow: auto;
		overflow-y: hidden;
	}
	.tophead{
		position: sticky;
		top: 0;
		z-index:999
	}
</style>
