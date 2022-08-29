<!-- <template>
	<view>
		<button type="default" @click="uploadphnoe">上传</button>
		<image v-for="(item,index) in imgList" :src="item" mode=""></image>
		<button type="default" @click="shanchuang()">上传2</button>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				imgList: [],
				// imglook: ""
			}
		},
		methods: {
			uploadphnoe() {
				uni.chooseImage({
					count: 1,
					success: (res) => {
						console.log(res);
						this.imgList = res.tempFilePaths
					}
				})
			},
			shangc() {
				const db = uniCloud.database();
				db.collection('dongtai').add({
					message: ' ',
					photo: this.imgList[0],
					look: 0,
					zan: 0,
					pinglu: 0
				}).then(() => {
					// console.log('成功');
					uni.showLoading({
						title: '加载中'
					})
					const db = uniCloud.database();
					db.collection('dongtai').get().then(res => {
						console.log(res.result.data.photo);
						this.imglook = res.result.data.photo
						uni.hideLoading()
					})
				})

			},
			shanchuang(){
				console.log("图片："+this.imgList[0]);
				// uni.
			},
			

		}
	}
</script>

<style>

</style>
 -->
 
 
 <template>
 	<view class="">
 		<button class="btn" type="default" @click="upload">上传图片</button>
 		<button class="btn" type="default" @click="browseImage">查看云图片</button>
 	</view>
 </template>
 
 <script>
 	export default {
 		data() {
 			return {}
 		},
 		methods: {
 			upload() {
 				uni.chooseImage({
 					count: 1,
 					success(res) {
 						console.log(res);
 						if (res.tempFilePaths.length > 0) {
 							uni.showLoading({
 								title: '上传中...'
 							})
 							
 							let filePath = res.tempFilePaths[0];
							console.log('filePath='+filePath);
 							// callback方式 ，进行上传操作
 							uniCloud.uploadFile({
 								filePath: filePath,  //要上传的文件对象
 								cloudPath: Date.now() + '.jpg',  //保存在云端的文件名，这里以时间戳命名
 								success(res) {
 									//console.log(res.fileID)
 									let imageUrl = res.fileID  //云端返回的图片地址
									console.log(imageUrl);
 									uniCloud.callFunction({  //调用云端函数，把图片地址写入表
 										name: 'addImage',  //云函数名称
 										data: {				//提交给云端的数据
 											imageUrl: imageUrl,									
 											createTime: Date.now()
 										},
 										success: (res) => {	
 											//console.log('数据插入成功')
 											console.log(res)
 										},
 										fail: (err) => {
 											console.log(err)
 										},
 										complete: () => {
 											
 										}				
 									})
 								},
 								fail(err) {
 									console.log(err)
 								},
 								complete() {
 									uni.hideLoading()
 								}
 							});		
 						}
 					}
 				});
 			},
 			
 			browseImage() {				
 				uni.navigateTo({  //跳转到指定页面
 					url: "../browseImage/browseImage",
 				});
 			}
 		}
 	}
 </script>
 
 <style>
 	.btn{
 		margin: 10px;
 		background-color: #4CD964;
 	}
 </style>
 
