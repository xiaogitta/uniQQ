<template>
	<view>
		<!-- 头导航 -->
		<view class="cu-bar bg-white search">
			<view class="cu-avatar round" style="background-image:url(../../static/yanxi.png);"></view>
			<view class="content">
				我的动态
			</view>
			<view class="action">
				<text class="cuIcon-more"></text>
			</view>
		</view>

		<view class="cu-card dynamic" v-for="(item,index) in dataList">
			<view class="cu-item shadow">
				<view class="cu-list menu-avatar" @touchstart.prevent="goTouchstart(item._id)"@touchend.prevent="goTouchend()">
					<view class="cu-item">
						<view class="cu-avatar round lg" style="background-image:url(../../static/yanxi.png);">
						</view>
						<view class="content flex-sub">
							<view>言溪</view>
							<view class="text-gray text-sm flex justify-between">
								Shaping Up
							</view>
						</view>
					</view>
				</view>
				<view class="text-content" @click="onlook()">
					{{item.message}}
				</view>
				<view class="grid flex-sub padding-lr" :class="isCard?'col-3 grid-square':'col-1'">
					<image :src="item.photo" ></image>
				</view>
				<!-- 点赞观看 -->
				<view class="text-gray text-sm text-right padding">
					<text class="cuIcon-attentionfill margin-lr-xs"></text> {{item.look}}
					<text class="cuIcon-appreciatefill margin-lr-xs" @click="dianzan(item._id)"></text> {{item.zan}}
					<text class="cuIcon-messagefill margin-lr-xs" @click="pinglun()"></text> {{item.pinglu}}
				</view>
				<br><br>
			</view>
		</view>
		<!-- 按钮 -->
		<button type="default" class="but but2" @click="toAddMon()">+</button>



		<view class="cu-bar tabbar bg-white footer">
			<view class="action text-gray">
				<view class="cuIcon-message" @click="toPages(0)"></view> 消息
			</view>
			<view class="action text-gray">
				<view class="cuIcon-addressbook" @click="toPages(1)"></view> 联系人
			</view>
			<!-- 	<view class="action text-gray">
				<view class="cuIcon-attention"@click="toPages(2)"></view> 看点
			</view> -->
			<view class="action text-green">
				<view class="cuIcon-share" @click="toPages(3)"></view> 动态
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				url: ["../message/message", "../contacts/contacts", "../QQKanDian/QQKanDian", "../Moments/Moments"],
				dataList: "",
				pinglu: 0,
				index: 0,
				modalName: null,
				insearch: "",
				isCard: false
			};
		},
		onLoad() {
			const db = uniCloud.database();
			db.collection('dongtai').get().then(res => {
				console.log(res);
				this.dataList = res.result.data
			})
		},
		methods: {
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
				this.toSearch()
			},
			hideModal(e) {
				this.modalName = null
			},
			IsCard(e) {
				this.isCard = e.detail.value
			},
			toPages(index) {
				uni.navigateTo({
					url: this.url[index]
				})
			},
			toAddMon() {
				uni.navigateTo({
					url: './addMoments'
				})
			},
			// onlook(e) {
			// 	this.look++
			// 	const db = uniCloud.database();
			// 	db.collection('dongtai').where({
			// 		id: e,
			// 	}).update({
			// 		look: this.look
			// 	})
			// },
			dianzan(e) {
				const db = uniCloud.database();
				db.collection('dongtai').where({
					_id: e,
				}).update({
					zan: 1,
					look:1
				}).then(() => {
					const db = uniCloud.database();
					db.collection('dongtai').get().then(res => {
						console.log(res);
						this.dataList = res.result.data
					})
				})
			},
			pinglun() {
				uni.showModal({
					title: '开发中' + '\n' + '敬请期待'
				})
			},
			toSearch(){
				
			},
			del(e) {
				console.log(e);
				uni.showModal({
					content:"确认删除！",
					success:(res)=>{
						// console.log(res);
						if(res.confirm){
							const db = uniCloud.database();
							db.collection('dongtai').doc(e).remove().then(()=>{
								const db = uniCloud.database();
								db.collection('dongtai').get().then(res=>{
									this.dataList = res.result.data
								})
							})
						}
					}
				})
			},
			goTouchstart(_id) {
				let _this = this;
				clearTimeout(_this.timeOutEvent);
				_this.timeOutEvent = setTimeout(function() {
					_this.timeOutEvent = 0;
					_this.del(_id)
				}, 1000);
			},
			//手如果在600毫秒内就释放，则取消长按事件
			goTouchend() {
			    let _this = this;
			    clearTimeout(_this.timeOutEvent);
			    if (_this.timeOutEvent !== 0) {
			        //  处理单击事件
			    }
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

	.but {
		background-color: #4CAF50;
		/* Green */
		border: none;
		color: white;
		padding: 0rpx 40rpx;
		text-align: center;
		text-decoration: none;
		display: inline-block;
		font-size: 50rpx;
		margin: 4rpx 2rpx;
		cursor: pointer;
		-webkit-transition-duration: 0.4s;
		/* Safari */
		transition-duration: 0.4s;
		border-radius: 50%;
		display: ;
		position: fixed;
		right: 50rpx;
		top: 1000rpx;
		z-index: 998;
	}

	.but2:hover {
		box-shadow: 0 12rpx 16rpx 0 rgba(0, 0, 0, 0.24), 0 17rpx 50rpx 0 rgba(0, 0, 0, 0.19);
	}
</style>
