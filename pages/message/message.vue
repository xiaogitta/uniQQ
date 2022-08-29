<template>
	<view>
		<view class="main-overflow">
			<view class="cu-bar search bg-white">
				<view class="cu-avatar round" style="background-image:url(../../static/yanxi.png" @tap="showModal"
					data-target="DrawerModalL">
				</view>
				<view class="search-form round">
					<text class="cuIcon-search"></text>
					<input type="text" placeholder="搜索图片、文章、联系人" v-model="insearch"></input>
				</view>
				<view class="action">
					<button class="cu-btn round sm bg-blue" @tap="showModal" data-target="bottomModal">搜索</button>
				</view>
			</view>
			<view class="cu-modal drawer-modal justify-start" :class="modalName=='DrawerModalL'?'show':''"
				@tap="hideModal">
				<view class="cu-dialog basis-lg" >
					<view class="cu-list menu text-left writLine">
						<br/>打工人很忙
					</view>
				</view>
			</view>




			<view class="cu-modal bottom-modal" :class="modalName=='bottomModal'?'show':''">
				<view class="cu-dialog">



					<view class="cu-bar search bg-white">
						<view class="cu-avatar round" style="background-image:url(../../static/yanxi.png">
						</view>
						<view class="search-form round">
							<text class="cuIcon-search"></text>
							<input type="text" placeholder="搜索图片、文章、联系人" v-model="insearch"></input>
						</view>
						<view class="action">
							<button class="cu-btn round sm bg-blue" @tap="showModal"
								data-target="bottomModal">搜索</button>
						</view>
					</view>




					<!-- 拟态框 -->
					<view class="cu-bar bg-white">
						<view class="action text-green" @tap="hideModal">确定</view>
						<view class="action text-blue" @tap="hideModal">取消</view>
					</view>
					<view class="padding-x1 modalFrame">
						<view class="cu-list menu-avatar">
							<view class="cu-item" @click="toquery()" v-for="(item,index) in dataList">
								<!-- <view class="cu-avatar round lg"
									style="background-image:url(https://ossweb-img.qq.com/images/lol/img/champion/Morgana.png);">
								</view> -->
								<image class="cu-avatar round lg" :src="item.touxiang">
								</image>
								<view class="content">
									<view class="text-grey">{{item.username}}</view>
									<view class="text-gray text-sm flex">
										<view class="text-cut">
											<text class="cuIcon-markfill text-red  margin-right-xs"></text>
											备注：{{item.remark}}
										</view>
									</view>
								</view>


							</view>
						</view>
					</view>
				</view>
			</view>





			<view class="cu-list menu-avatar radius">
				<view class="cu-item" @click="toWechat()">
					<view class="cu-avatar round lg"
						style="background-image:url(https://ossweb-img.qq.com/images/lol/web201310/skin/big10006.jpg);">
					</view>
					<view class="content">
						<view class="text-grey">测试</view>
						<view class="text-gray text-sm">
							<text class="cuIcon-moneybag text-red"></text>
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<view class="cu-tag round bg-gradual-red sm">5</view>
					</view>
				</view>
				<view class="cu-item" @click="toWechat()">
					<view class="cu-avatar round lg" style="background-image:url(../../static/yanxi2.png);">
					</view>
					<view class="content">
						<view class="text-grey">小橙
							<view class="cu-tag round sm">SVIP</view>
						</view>
						<view class="text-gray text-sm">
							<text class="cuIcon-redpacket_fill text-red"></text> 收到红包
						</view>
					</view>
					<view class="action">
						<view class="text-grey text-xs">22:20</view>
						<text class="cu-tag round bg-gradual-red sm">99+</text>
					</view>
				</view>
			</view>


		</view>

		<view class="cu-bar tabbar bg-white footer">
			<view class="action text-green">
				<view class="cuIcon-message" @click="toPages(0)"></view> 消息
			</view>
			<view class="action text-gray">
				<view class="cuIcon-addressbook" @click="toPages(1)"></view> 联系人
			</view>
			<!-- 		<view class="action text-gray">
				<view class="cuIcon-attention" @click="toPages(2)"></view> 看点
			</view> -->
			<view class="action text-gray">
				<view class="cuIcon-share" @click="toPages(3)"></view> 动态
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				modalName: null,
				url: ["../message/message", "../contacts/contacts", "../QQKanDian/QQKanDian", "../Moments/Moments"],
				insearch: "",
				dataList: []
			}
		},
		methods: {
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
				this.toSearch()
			},
			hideModal(e) {
				this.modalName = null
			},
			toPages(index) {
				uni.navigateTo({
					url: this.url[index]
				})
			},
			toWechat() {
				uni.navigateTo({
					url: './wechat'
				})
			},
			toSearch() {
				this.dataList = []
				if (this.insearch != "") {
					const db = uniCloud.database();
					db.collection('tongxunlu').get().then(res => {
						res.result.data.forEach((item, index) => {
							// if (item.username == this.insearch) 
							if (item.username.indexOf(this.insearch) != -1) {
								this.dataList.push(item)
							}
						})

					})
				}

			},
			toquery() {
				uni.navigateTo({
					url: "../contacts/query"
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

	.modalFrame {
		height: 9999rpx;
	}

	.writLine {
		writing-mode: vertical-lr;
		height: 1000rpx;
		font-size: 90rpx;
		font-weight: 100;
		width: auto;
		margin-top: 100rpx;
		float: left;
		font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
		color: #999999;
		line-height: 100rpx;
		letter-spacing: 10rpx
	}
</style>
