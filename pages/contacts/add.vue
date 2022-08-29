<template>
	<view>
		<!-- 返回导航 -->
		<view class="cu-bar bg-white solid-bottom tophead">
			<view class="action" @click="toBack()">
				<text class="cuIcon-back text-progress"></text>返回
			</view>
		</view>
		<view class="cu-bar bg-white">
			<view class="action">
				<text class="cuIcon-addressbook text-yellow"></text> 通讯录表单
			</view>
		</view>
		<view class="cu-form-group">
			<view class="title">姓名</view>
			<input placeholder="请输入姓名" name="input" v-model="Name"></input>
			<text class='cuIcon-people text-orange'></text>
		</view>
		<view class="cu-form-group">
			<view class="title">手机</view>
			<input placeholder="请输入手机号码" name="input" v-model="Phone"></input>
			<view class="cu-capsule radius">
				<view class='cu-tag bg-yellow'>
					+86
				</view>
				<view class="cu-tag line-yellow">
					中国大陆
				</view>
			</view>
		</view>
		<view class="cu-form-group">
			<view class="title">备注</view>
			<input placeholder="请输入备注" name="input" v-model="Remark"></input>
			<text class='cuIcon-mark text-orange'></text>
		</view>
		<view class="padding flex flex-direction">
			<button class="cu-btn bg-green margin-tb-sm lg" @click="save()">保存</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				Name: "",
				Phone: "",
				Remark: ""
			}
		},
		methods: {
			save() {
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
					// 数据库保存
					const db = uniCloud.database();
					db.collection('tongxunlu').add({
						username: this.Name,
						phone: this.Phone,
						remark: this.Remark,
						touxiang:"../../static/yanxi.png"
					}).then(res => {
						// console.log(res);
						if (res.result.code == 0) {
							uni.showToast({
								title: '保存成功',
								duration: 1500,
								mask: true,
								icon: 'success'
							})
						} else {
							uni.showToast({
								title: '保存失败',
								duration: 1500,
								mask: true,
								icon: 'error'
							})
						}
						//退回清空页面
						setTimeout(() => {
							uni.navigateBack({
								delta: 1
							}, 2500)
						})
					})
				}

			},
			toBack() {
				uni.navigateBack({
					delta: 1
				})
			}

		}
	}
	// 数据添加
	// const db = uniCloud.database();	
	// db.collection('tongxunlu').add({username:"王五",phone:"15577169533",remark:"好友"});
	// console.log(db);
</script>

<style>
	.cu-form-group .title {
		min-width: calc(4em + 15px);
	}
</style>
