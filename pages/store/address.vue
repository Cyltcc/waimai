<template>
	<view class="d-clo d-ai-centen">
		
	
			<view class=" width95 height200 margin-top-20 bg-color-white d-row d-jc-between d-ai-centen ">
				<view class="d-clo d-jc-around height180 margin-20">
					<view class="font-bold font36">水库新村</view>
					<view>5街六栋三单元301</view>
					<view class="gray3">林(先生) 159****9963</view>
				</view>
				<view @click="editaddress" class="margin-30"> <u-icon name="edit-pen" size="36" color="#c6c6c6"></u-icon></view>
			</view>
			<u-button style="margin-top: 100rpx;" @click="add">
				新增收货地址
			</u-button>
		
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			} 
		},
		methods: {
			editaddress() {
				console.log("修改地址")
				uni.navigateTo({
					url: './editaddress',
					animationType: 'pop-in',
					animationDuration: 2000
				})
			},
			add() {
				uni.navigateTo({
					url: './createaddress',
					animationType: 'pop-in',
					animationDuration: 2000
				})
			}
		}
	}
</script>

<style>
page{
	background-color: #f7f7f7;
}
</style>
