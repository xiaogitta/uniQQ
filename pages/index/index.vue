<template>
	<view class="content"@click="show()">
		<image class="logo" src="/static/touxiang.jpeg"></image>
		<view class="text-area">
			<text class="title">{{title}}</text>
		</view>
		<view class="text-area">
			<text class="title">{{mgs}}</text>
		</view>
	</view>
</template>
<script>
	export default {
		data() {
			return {
				title: 'Hello QQ',
				mgs:'小橙',
			}
		},
		onLoad() {
			var _this=3
			setInterval(function(){
				_this--
				if(_this==0){
					uni.navigateTo({
						url:'../message/message'
					})
				}
			},1000) 
		},
		methods: {
			show(){
				uni.navigateTo({
					url:"../message/message",
					animationType:'pop-in',
					animationDuration:2000
				})
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
		height: 300rpx;
		width: 300rpx;
		margin-top: 300rpx;
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
