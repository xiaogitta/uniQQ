<template>
	<view>
		<!-- 返回导航 -->
		<view class="cu-bar bg-white solid-bottom tophead">
			<view class="action" @click="toBack()">
				<text class="cuIcon-back text-progress"></text>返回
				<text class=""></text>
			</view>
		</view>
		<view class="cu-chat">
			<view class="cu-item self">
				<view class="main">
					<view class="content bg-green shadow">
						<text>喵喵喵！喵喵喵！喵喵喵！喵喵！喵喵！！喵！喵喵喵！</text>
					</view>
				</view>
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi.png);">
				</view>
				<view class="date">2018年3月23日 13:23</view>
			</view>
			<view class="cu-info round">对方撤回一条消息!</view>
			<view class="cu-item">
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi2.png);">
				</view>
				<view class="main">
					<view class="content shadow">
						<text>喵喵喵！喵喵喵！</text>
					</view>
				</view>
				<view class="date "> 13:23</view>
			</view>
			<view class="cu-info">
				<text class="cuIcon-roundclosefill text-red "></text> 对方拒绝了你的消息
			</view>
			<view class="cu-info">
				对方开启了好友验证，你还不是他(她)的好友。请先发送好友验证请求，对方验证通过后，才能聊天。
				<text class="text-blue">发送好友验证</text>
			</view>
			<view class="cu-item self">
				<view class="main">
					<image src="../../static/yanxi.png" class="radius" mode="widthFix"></image>
				</view>
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi.png);">
				</view>
				<view class="date"> 13:23</view>
			</view>
			<view class="cu-item self">
				<view class="main">
					<view class="action text-bold text-grey">
						3"
					</view>
					<view class="content shadow">
						<text class="cuIcon-sound text-xxl padding-right-xl"> </text>
					</view>
				</view>
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi.png);">
				</view>
				<view class="date">13:23</view>
			</view>
			<view class="cu-item self">
				<view class="main">
					<view class="action">
						<text class="cuIcon-locationfill text-orange text-xxl"></text>
					</view>
					<view class="content shadow">
						喵星球，喵喵市
					</view>
				</view>
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi.png);">
				</view>
				<view class="date">13:23</view>
			</view>
			<view class="cu-item">
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi2.png);">
				</view>
				<view class="main">
					<view class="content shadow">
						@#$^&**
					</view>
					<view class="action text-grey">
						<text class="cuIcon-warnfill text-red text-xxl"></text> <text
							class="text-sm margin-left-sm">翻译错误</text>
					</view>
				</view>
				<view class="date">13:23</view>
			</view>

			<!-- 修改内容 -->
			<view class="cu-item self" v-for="(item,index) in sites" >
				<view class="main">
					<!-- 绑定长按 -->
					<view class="content bg-green shadow"@touchstart.prevent="goTouchstart(item._id)"@touchend.prevent="goTouchend()">
						<text>{{item.message}}</text>
					</view>
				</view>
				<view class="cu-avatar radius" style="background-image:url(../../static/yanxi.png);">
				</view>
			</view>


		</view>


		<view class="cu-bar foot input" :style="[{bottom:InputBottom+'px'}]">
			<view class="action">
				<text class="cuIcon-sound text-grey"></text>
			</view>
			<input class="solid-bottom" :adjust-position="false" :focus="false" maxlength="300" cursor-spacing="10"
				@focus="InputFocus" @blur="InputBlur" v-model="message"></input>
			<view class="action">
				<text class="cuIcon-emojifill text-grey"></text>
			</view>
			<button class="cu-btn bg-green shadow" @click="add()">发送</button>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				InputBottom: 0,
				sites: [],
				message: "",
				num: 0
			};
		},
		onLoad() {
			const db = uniCloud.database();
			db.collection('message').get().then(res => {
				// console.log(res)
				this.sites = res.result.data
			}).then(() => {
				uni.pageScrollTo({
					scrollTop: 99999,
					duration: 10
				})
			})
		},
		timeOutEvent: 0,
		methods: {
			InputFocus(e) {
				this.InputBottom = e.detail.height
			},
			InputBlur(e) {
				this.InputBottom = 0
			},
			toBack() {
				uni.navigateTo({
					url:"./message"
				})
			},
			add() {
				if(this.message!=""){
					const db = uniCloud.database();
					db.collection('message').add({
						message: this.message
					})
					setTimeout(() => {
						uni.navigateTo({
							url: 'wechat'
						}, 1500)
					})
				}	
			},
			del(e) {
				console.log(e);
				uni.showModal({
					content:"确认删除！",
					success:(res)=>{
						// console.log(res);
						if(res.confirm){
							const db = uniCloud.database();
							db.collection('message').doc(e).remove().then(()=>{
								const db = uniCloud.database();
								db.collection('message').get().then(res=>{
									this.sites = res.result.data
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
	page {
		padding-bottom: 100upx;
	}
</style>
