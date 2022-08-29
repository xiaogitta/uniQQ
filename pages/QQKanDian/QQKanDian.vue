<template>
	<view>
		<view class="cu-bar search bg-white">
			<view class="cu-avatar round" style="background-image:url(../../static/yanxi.png)"></view>
			<view class="search-form round">
				<text class="cuIcon-search"></text>
				<input @focus="InputFocus" @blur="InputBlur" :adjust-position="false" type="text" placeholder="搜索图片、文章、视频" confirm-type="search"></input>
			</view>
			<view class="action">
				<text>广州</text>
			</view>
		</view>
		<video id="myvideo" src="https://vkceyugu.cdn.bspapp.com/VKCEYUGU-768239a0-bd59-44ca-9447-521e4db272fb/624ee7d0-ae7f-4874-beba-ed933429b38e.mp4" autoplay="true"
		controls="false"page-gesture="true"></video>
		
		
		
		<view class="cu-bar tabbar bg-white footer">
			<view class="action text-gray">
				<view class="cuIcon-message"@click="toPages(0)"></view> 消息
			</view>
			<view class="action text-gray">
				<view class="cuIcon-addressbook"@click="toPages(1)"></view> 联系人
			</view>
			<view class="action text-green">
				<view class="cuIcon-attention"@click="toPages(2)"></view> 看点
			</view>			
			<view class="action text-gray">
				<view class="cuIcon-share"@click="toPages(3)"></view> 动态
			</view>	
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				url:["../message/message","../contacts/contacts","../QQKanDian/QQKanDian","../Moments/Moments"],		
	
			}
		},
		methods: {
			toPages(index){
				uni.navigateTo({
					url:this.url[index]
				})
			}
			
		}
	}
</script>

<style>
	.box {
		margin: 20upx 0;
	}
	
	.box view.cu-bar {
		margin-top: 20upx;
	}


</style>
