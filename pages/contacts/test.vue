<!-- <template>
	<view>
		
		<view v-for="(item,index) in dataList" :key="index" class="cu-bar bg-white margin-top">
			{{item.username}}
			<button type="default" @click="toDelete(item._id)">删除</button>
			<view class="action">
				<button class="cu-btn bg-green shadow" @tap="showModal" data-target="Modal">Modal</button>
			</view>
			<view class="cu-modal" :class="modalName=='Modal'?'show':''">
				<view class="cu-dialog">
					<view class="cu-bar bg-white justify-end">
						<view class="content">Modal标题</view>
						<view class="action" @tap="hideModal">
							<text class="cuIcon-close text-red"></text>
						</view>
					</view>
					<view class="padding-xl">
						Modal 内容。
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
				modalName: null,
			}
		},
		
		methods: {

		}
	}
</script>

<style>

</style>
 -->
<template>
	<view>
	
		<view class="cu-bar bg-white margin-top" v-for="(item,index) in dataList">
			
			
			<view class="action">
				<text class="cuIcon-title text-orange "></text>{{item.username}}{{index}}
			</view>
			<view class="action" @click="toIndex(index)">
				<button class="cu-btn bg-green shadow" @tap="showModal" data-target="Modal">Modal</button>
			</view>
		</view>
		
		
		<view class="cu-modal" :class="modalName=='Modal'?'show':''">
			<view class="cu-dialog">
				<view class="cu-bar bg-white justify-end">
					<view class="content">修改</view>
					<view class="action" @tap="hideModal">
						<text class="cuIcon-close text-red"></text>
					</view>
				</view>
				<view class="padding-xl">
					
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
				num:0,
				dataList: '',
				modalName: null,
			};
		},
		onLoad() {
			uni.showLoading({
				title: '加载中'
			})
			const db = uniCloud.database();
			db.collection('tongxunlu').get().then(res => {
				console.log(res);
				this.dataList = res.result.data
				uni.hideLoading()
			})
		},
		methods: {
			
			toDelete(e) {
				//删除对应id值的数据记录
				uniCloud.database().collection('tongxunlu').doc(e).remove().then(res => {
					if (res.errCode == 0) {
						uni.showToast({
							title: '删除成功',
							duration: 1500
						})
					}
				}).then(() => {
					uniCloud.database().collection('tongxunlu').get().then(res => {
						this.dataList = res.result.data
					})
				})

			},
			toUpdate(e) {
				//修改表中id为5f79fdb337d16d0001899566的记录
				uniCloud.database().collection("tongxunlu").where({
					_id: e
				}).update({
					username: "小红",
					phone: "13978125458",
					remark: "亲戚",
				});
			},
			showModal(e) {
				this.modalName = e.currentTarget.dataset.target
			},
			hideModal(e) {
				this.modalName = null
			},
			toIndex(index){
				this.num=index
				console.log(12222222222222);
				
			},
		}
	}
</script>

<style>
	.cu-form-group .title {
		min-width: calc(4em + 15px);
	}

</style>
