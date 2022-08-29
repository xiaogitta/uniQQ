<template>
	<view>
		<!-- 返回导航 -->
		<view class="cu-bar bg-white solid-bottom tophead">
			<view class="action" @click="toBack()">
				<text class="cuIcon-back text-progress"></text>返回
				<text class=""></text>
			</view>
		</view>
		<form>
			<view class="cu-form-group margin-top">
				<textarea maxlength="-1" @input="textareaAInput" placeholder="浅聊一下留言板的回忆" v-model="message"></textarea>
			</view>
			<view class="cu-bar bg-white margin-top">
				<view class="action">
					图片上传
				</view>
				<view class="action">
					{{imgList.length}}/1
				</view>
			</view>
			<view class="cu-form-group">
				<view class="grid col-4 grid-square flex-sub">
					<view class="bg-img" v-for="(item,index) in imgList" :key="index" @tap="ViewImage"
						:data-url="imgList[index]">
						<image :src="imgList[index]" mode="aspectFill"></image>
						<view class="cu-tag bg-red" @tap.stop="DelImg" :data-index="index">
							<text class='cuIcon-close'></text>
						</view>
					</view>
					<view class="solids" @tap="ChooseImage" v-if="imgList.length<2">
						<text class='cuIcon-cameraadd'></text>
					</view>
				</view>
			</view>
		</form>
		<view class="cu-bar btn-group footer hhh">
			<button class="cu-btn bg-green shadow-blur round lg" @click="toSave()">分享</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				index: -1,
				imgList: [],
				message: "",
				imgUrl: ""
			};
		},
		methods: {
			toBack() {
				uni.navigateBack({
					delta: 1
				})
			},
			ChooseImage() {
				uni.chooseImage({
					count: 4, //默认9
					sizeType: ['original', 'compressed'], //可以指定是原图还是压缩图，默认二者都有
					sourceType: ['album'], //从相册选择
					success: (res) => {
						console.log(res);
						if (this.imgList.length != 0) {
							this.imgList = this.imgList.concat(res.tempFilePaths)
						} else {
							this.imgList = res.tempFilePaths
						}
					}
				});
			},
			ViewImage(e) {
				uni.previewImage({
					urls: this.imgList,
					current: e.currentTarget.dataset.url
				});
			},
			DelImg(e) {
				uni.showModal({
					title: '召唤师',
					content: '确定要删除这段回忆吗？',
					cancelText: '再看看',
					confirmText: '再见',
					success: res => {
						if (res.confirm) {
							this.imgList.splice(e.currentTarget.dataset.index, 1)
						}
					}
				})
			},
			textareaAInput(e) {
				this.textareaAValue = e.detail.value
			},
			toSave() {
				if (this.message == '') {
					uni.showToast({
						title: '请输入内容',
						icon: 'none'
					})
				} else {
					uni.showLoading({
						title: '上传中...',
					})
					let msg = this.message //获取message 无解？uploadFile内无法获取外部message
					uniCloud.uploadFile({
						filePath: this.imgList[0], //要上传的文件对象
						cloudPath: Date.now() + '.jpg', //保存在云端的文件名，这里以时间戳命名
						success(res) {
							//console.log(res.fileID)
							let imageUrl = res.fileID //云端返回的图片地址
							// console.log(imageUrl);
							// console.log('接口内----------------------');
							const db = uniCloud.database();
							db.collection('dongtai').add({
								// message: this.message, // Why无法直接获取message
								message: msg,
								photo: imageUrl, //数据库添加图片地址
								look: 0,
								zan: 0,
								pinglu: 0
							}).then(()=>{
								uni.showToast({
									title: '上传成功',
									duration: 1500,
									mask: true,
									icon: 'success'
								});
							}).then(()=>{
								uni.navigateTo({
									url: 'Moments'
								})
							})
							// console.log("0000"+this.message);
						},
						fail(err) {
							console.log(err)
						},
						complete() {
							uni.hideLoading()
						}
					});					
					// console.log('接口外----------------');
				}
			}
		}
	}
</script>

<style>
	.cu-form-group .title {
		min-width: calc(4em + 15px);
	}

	.hhh {
		margin-bottom: 100rpx;
	}
</style>
