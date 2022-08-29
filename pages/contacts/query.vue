<template>
	<view>
		<!-- 返回导航 -->
		<view class="cu-bar bg-white solid-bottom tophead">
			<view class="action" @click="toBack()">
				<text class="cuIcon-back text-progress"></text>返回
			</view>
		</view>
		<!-- 通讯录 -->
		<view class="cu-bar bg-white">
			<view class="action">
				<text class="cuIcon-taxi text-red"></text> 通讯录列表
			</view>
		</view>
		<view class="cu-list menu-avatar" v-for="(item,index) in dataList">
			<view class="cu-item" @click="toIndex(index)">
				<!-- <view class="cu-avatar round lg"
					style="background-image:url(../../static/yanxi.png);">
				</view> -->
				<image class="cu-avatar round lg" :src="item.touxiang" @tap="showModal" data-target="DialogModal1">
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
				<view class="action" @click="toIndex(index)">
					<button class="cu-btn round sm bg-orange" @tap="showModal" data-target="Modal">修改</button>
				</view>
				<view class="action">
					<button class="cu-btn round sm bg-red" @click="toDelete(item._id)">删除</button>
				</view>
			</view>
		</view>

		<!-- 拟态窗口 -->
		<view class="cu-modal" :class="modalName=='Modal'?'show':''">
			<view class="cu-dialog">
				<view class="cu-bar bg-white justify-end">
					<view class="content">修改</view>
					<view class="action" @tap="hideModal">
						<text class="cuIcon-close text-red"></text>
					</view>
				</view>
				<view class="padding-xl">
					<view class="cu-form-group">
						<view class="title">姓名</view>
						<input placeholder="请输入姓名" name="input" v-model="Name"></input>
						<text class='cuIcon-people text-orange'></text>
					</view>
					<view class="cu-form-group">
						<view class="title">手机</view>
						<input placeholder="请输入手机号码" name="input" v-model="Phone"></input>
						<text class='cuIcon-phone text-orange'></text>
					</view>
					<view class="cu-form-group">
						<view class="title">备注</view>
						<input placeholder="请输入备注" name="input" v-model="Remark"></input>
						<text class='cuIcon-mark text-orange'></text>
					</view>

					<view class="padding flex flex-direction">
						<button class="cu-btn bg-green margin-tb-sm lg" @click="toUpdate(dataList[num]._id)">保存</button>
					</view>

				</view>
			</view>
		</view>
		<!-- 修改头像 -->
		<view class="cu-modal" :class="modalName=='DialogModal1'?'show':''">
			<view class="cu-dialog">
				<view class="cu-bar bg-white justify-end">
					<view class="content">修改头像</view>
					<view class="action" @tap="hideModal">
						<text class="cuIcon-close text-red"></text>
					</view>
				</view>
				<view class="padding-xl">
					<form>
						<view class="cu-form-group">
							<view class="grid col-2 grid-square flex-sub">
								<view class="div-child" v-for="(item,index) in imgList" :key="index" @tap="ViewImage"
									:data-url="imgList[index]">
									<image :src="imgList[index]" mode="aspectFill"></image>
								</view>
								<view class="solids div-child" @tap="ChooseImage" v-if="imgList.length<1">
									<text class='cuIcon-cameraadd'></text>
								</view>
							</view>
						</view>
					</form>
					<view class="cu-bar btn-group">
						<button class="cu-btn bg-green shadow-blur round lg"@click="toSaveImg(dataList[num]._id)">保存</button>
					</view>
				</view>
			</view>
		</view>


	</view>
</template>

<script>
	export default {
		data() {
			return {
				dataList: '',
				num: 0,
				modalName: null,
				_id: '',
				Name: "",
				Phone: "",
				Remark: "",
				imgsrc: ["../../static/yanxi.png", "../../static/wq2.jpg"],

				index: -1,
				imgList: [],
				message: "",
				imgUrl: ""
			}
		},
		onLoad() {
			// uni.showLoading({
			// 	title: '加载中'
			// })
			const db = uniCloud.database();
			db.collection('tongxunlu').get().then(res => {
				// console.log(res);
				this.dataList = res.result.data
				// uni.hideLoading()
			})

		},
		methods: {
			toDelete(e) {
				uni.showModal({
					content: '是否确认删除',
					success: (res) => {
						if (res.confirm) {
							const db = uniCloud.database();
							db.collection('tongxunlu').doc(e).remove().then(res => {
								if (res.result.code == 0) {
									uni.showToast({
										title: '删除成功',
										duration: 1500,
										icon: 'success'
									})
								}
							}).then(() => {
								const db = uniCloud.database();
								db.collection('tongxunlu').get().then(res => {
									this.dataList = res.result.data
								})
							})
						}
					}
				})
			},
			toUpdate(e) {
				if (this.Phone == "") {
					uni.showToast({
						title: '请输入电话',
						icon: 'none'
					})
				}
				if (this.Name == "") {
					uni.showToast({
						title: '请输入姓名',
						icon: 'none'
					})
				}
				if (this.Name != "" && this.Phone != "") {
					uni.showLoading({
						title: '保存中'
					})
					uni.showModal({
						content: '确认修改',
						success: (res) => {
							if (res.confirm) {
								uniCloud.database().collection("tongxunlu").where({
									_id: e
								}).update({
									username: this.Name,
									phone: this.Phone,
									remark: this.Remark,
								}).then(() => {
									const db = uniCloud.database();
									db.collection('tongxunlu').get().then(res => {
										this.dataList = res.result.data
									})
								}).then(() => {
									uni.showToast({
										title: '修改成功',
										duration: 1000,
										icon: 'success'
									}).then(()=>{
										setTimeout(()=>{
											uni.navigateTo({
												url:'query'
											})
										},100)
									})
								})
							} else if (res.cancel) {
								uni.hideLoading()
							}
						}

					})
				}
				// console.log(e);
				this.hideModal(e)
			},

			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			toIndex(index) {
				this.num = index;
				this._id = this.dataList[index]._id;
				console.log(this._id);
			},
			toBack() {
				uni.navigateTo({
					url:'contacts'
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
			textareaAInput(e) {
				this.textareaAValue = e.detail.value
			},
			toSaveImg(e) {
				this.hideModal();
				uni.showLoading({
					title:'保存中...'
				})
				uniCloud.uploadFile({
					filePath: this.imgList[0], //要上传的文件对象
					cloudPath: Date.now() + '.jpg', //保存在云端的文件名，这里以时间戳命名
					success(res) {
						let imageUrl = res.fileID //云端返回的图片地址
						console.log(imageUrl);
						const db = uniCloud.database();
						db.collection('tongxunlu').where({
								_id: e
							}).update({
								touxiang:imageUrl
							}).then(()=>{
								uni.showToast({
									title:'保存成功！',
									mask:true,
								})
							}).then(() => {
								uni.navigateTo({
									url:'query'
								})
							})
					},
					fail(err) {
						console.log(err)
					},
				});
			}

		}
	}
</script>

<style>
	input {
		text-align: left;
		min-width: calc(4em + 15px);
	}

	.div-child {
		margin-left: 130rpx;
	}
</style>
