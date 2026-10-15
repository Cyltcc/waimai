<template>
	<view class="width100 d-clo d-jc-centen">
		<view class="width90 margin-center font36 gray3">基础信息</view>
		
		<view class="width90 height140 line-height140 d-row d-jc-between d-ai-start margin-center border-bottom-2 ">
			<view class="font32"> 头像 </view>
			<view class="d-row d-ai-centen gray3 height120 line-height120"> 
				<image class="head-img cell-list" src="../../static/avatar.png"></image>
				<u-icon name="arrow-right" ></u-icon> 
			</view>
		</view>
		
		<view class="width90 height140 line-height140 d-ai-centen d-row d-jc-between d-ai-start margin-center border-bottom-2  ">
			<view class="font32"> 用户名 </view>
			<view class="d-row d-ai-centen gray3  height120 line-height120 "> 
				<span class="cell-list">张三</span>
				<u-icon name="arrow-right" ></u-icon> 
			</view>
		</view>
		
		<navigator url="../store/address">
			<view class="width90 height140 line-height140 d-ai-centen d-row d-jc-between d-ai-start margin-center border-bottom-2 ">
				<view class="font32"> 收货地址 </view>
				<view class="gray3">
					<u-icon name="arrow-right" ></u-icon> 
				</view>
			</view>
		</navigator>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			
		}
	}
</script>

<style>
.head-img{
	width: 100rpx;
	height: 100rpx;
	border-radius: 50%;
}
.cell-list {
	margin-right: 25rpx;
}
</style>
