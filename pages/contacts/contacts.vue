<template>
	<view>
		<!-- 头导航 -->
		<view class="cu-bar bg-white search">
			<view class="cu-avatar round" style="background-image:url(../../static/yanxi.png);"></view>
			<view class="content">
				联系人
			</view>
			<view class="action">
				<text class="cuIcon-more"></text>
			</view>
		</view>
		
		<view>
			<swiper class="screen-swiper square-dot" :indicator-dots="true" :circular="true" :autoplay="true"
				interval="5000" duration="500">
				<swiper-item v-for="(item,index) in swiperList" :key="index">
					<image :src="item" mode="aspectFill"></image>
				</swiper-item>
			</swiper>
			<view class="cu-bar bg-white margin-top">
				<view class="action">
					<text class="cuIcon-title text-pink"></text> 通讯录功能
				</view>
			</view>
			<view class="cu-list menu sm-border card-menu margin-top">
				<view class="cu-item arrow">
					<view class="content"@click="add()">
						<text class="cuIcon-list text-yellow"></text>
						<text class="text-grey">添加通讯录</text>
					</view>
				</view>
				<view class="cu-item arrow">
					<view class="content"@click="query()">
						<text class="cuIcon-searchlist text-red"></text>
						<text class="text-grey">查询通讯录</text>
					</view>
				</view>
			</view>
		</view>
		
		
		
		<!-- 底部导航 -->
		<view class="cu-bar tabbar bg-white footer">
			<view class="action text-gray">
				<view class="cuIcon-message"@click="toPages(0)"></view> 消息
			</view>
			<view class="action text-green">
				<view class="cuIcon-addressbook"@click="toPages(1)"></view> 联系人
			</view>
<!-- 			<view class="action text-gray">
				<view class="cuIcon-attention"@click="toPages(2)"></view> 看点
			</view>		 -->	
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
				index: -1,
				url:["../message/message","../contacts/contacts","../QQKanDian/QQKanDian","../Moments/Moments"],
				imgList: [],
				swiperList: [
					'https://vkceyugu.cdn.bspapp.com/VKCEYUGU-768239a0-bd59-44ca-9447-521e4db272fb/8deb8eee-f062-41a2-b7df-568af28d964e.jpg',
					'https://vkceyugu.cdn.bspapp.com/VKCEYUGU-768239a0-bd59-44ca-9447-521e4db272fb/71796676-4f17-4960-bea2-a8d7a2382a00.jpg',
					'https://vkceyugu.cdn.bspapp.com/VKCEYUGU-768239a0-bd59-44ca-9447-521e4db272fb/33e9284b-95e9-453b-b017-3e4fbb4e1a40.jpg',
					'https://vkceyugu.cdn.bspapp.com/VKCEYUGU-768239a0-bd59-44ca-9447-521e4db272fb/f0ef02b8-a5cf-4323-af81-8e549c12ee6e.jpg'
				],
			}
		},
		methods: {
			toPages(index){
				uni.navigateTo({
					url:this.url[index]
				})
			},
			add(){
				uni.navigateTo({
					url:"./add"
				})
			},
			query(){
				uni.navigateTo({
					url:"./query"
				})
			}
			
		}
	}
	
</script>

<style>
	.box {
		margin: 20urpx 0;
	}
	
	.box view.cu-bar {
		margin-top: 20urpx;
	}
	.dropbtn {
	    background-color: #fdfdfd;
	    color: black;
	    padding: 16 rpx;
	    font-size: 16 rpx;
	    border: none;
	    cursor: pointer;
	}
	
	.dropdown {
	    position: relative;
	    display: inline-block;
	}
	
	.dropdown-content {
	    display: none;
	    position: absolute;
	    background-color: #f9f9f9;
	    min-width: 160rpx;
	    /* box-shadow: 0rpx 8rpx 16rpx 0rpx rgba(0,0,0,0.2); */
	}
	
	.dropdown-content a {
	    color: black;
	    padding: 12rpx 16rpx;
	    text-decoration: none;
	    display: block;
	}
	
	.dropdown-content a:hover {background-color: #f1f1f1}
	
	.dropdown:hover .dropdown-content {
	    display: block;
	}
	
	.dropdown:hover .dropbtn {
	    background-color: #ffff7f;
	}


</style>
