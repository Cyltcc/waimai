<template>
	<view class="width90 margin-center">
		
		<!-- 配送地址 -->
		<view class="bg-color-white padding-20 border-radius-15 margin-top-20">
			<view class="font36 font-bold" @tap="addressClick">
				<label>水库新村 5街六栋三单元301</label>
				<span style="float: right;"><u-icon name="arrow-right" color="#c1c1c1" size="28"></u-icon></span> 
			</view>
			<view class="margin-top-10">林(先生) 159****9963 --{{ this.$store.state.name }}</view>
			
			<view class="d-row d-jc-between margin-top-20">
				<view class="width45"><button plain :type="butt?'primary':''" @tap="butt = !butt">
					<u-icon :name="butt?'car-fill':'car'" size="30"></u-icon>外卖配送
				</button> </view>
				
				<view class="width45"><button plain :type="butt?'':'primary'" @tap="butt = !butt">
					<u-icon :name="butt?'car':'car-fill'" size="30"></u-icon>门店自取
				</button> </view>
			</view>
		</view>
		
		
		<!-- 支付方式 -->
		<view class="bg-color-white padding-20 border-radius-15 margin-top-20 font-bold font30">
			<view class="d-row d-jc-between padding-10 d-ai-centen" style="border-bottom: 1rpx solid #F7F7F7;">
				<view>立即送出</view>
				<view class="d-jc-between d-row">
					<view class="colorBlue">约 11:40 送达</view>
					<u-icon name="arrow-right" color="#c1c1c1" style="margin-left: 10px;" size="18"></u-icon>
				</view>
			</view>
			<view class="d-row d-jc-between d-ai-centen padding-10">
				<view>支付方式</view>
				<view class="d-jc-between d-row d-ai-centen">
					<view class="colorBlue">支付宝</view>
					<u-icon name="arrow-right" color="#c1c1c1" style="margin-left: 10px;" size="18"></u-icon>
				</view>
			</view>
		</view>
		
		
		<!-- 商品清单 -->
		<view class="bg-color-white padding-20 border-radius-15 margin-top-20 margin-bottom-200">
			<view class="d-row d-jc-between padding-10 padding-bottom-20 border-bottom-2"
				v-for="(item,index) in cart" :key="index">
				<view class="d-row">
					<view class="d-clo d-ai-centen">  <image style="width: 80rpx;height: 80rpx;" src="../../static/coffee.png" ></image> </view>
					<view class="d-clo d-jc-between padding-left-10"> <h3>{{item.name}}</h3> <label class="font22 gray4">{{item.type}}</label> </view>
				</view>
				<view>
					<span >X {{item.number}}</span>
					<span class="padding-40">¥{{item.price}}</span>
				</view>
			</view>
			
			
			<view class="d-row d-jc-between padding-10 font30 padding-bottom-20 border-bottom-2">
				<view>配送费</view>
				<view>¥2</view>
			</view>
			
			<view class="d-row d-jc-between d-ai-centen padding-10 padding-top-30">
				<view class="gray3" @tap="privilegeClick"> 优惠说明<u-icon name="question-circle" ></u-icon></view>
				<view>共{{ numsum }}个商品，小计：<label class="font-bold font40">¥ {{ total }}</label></view>
			</view>
			
		</view>
		
		<!-- 去支付 -->
		<view class="pay">
			<view style="margin:0 auto;" class="width90 d-row d-jc-between d-ai-centen">
				<view class="d-row d-jc-start d-ai-centen" style="height: 120rpx;padding-left: 10rpx;" > 
					<view class="colorWhite font60 padding-left-10">
						<span class="font40">¥</span>
						<span class="padding-left-10">{{ total }}</span>
					</view>
				</view>
				<view class="go gray4">
					<button class="go gray4 font28 colorWhiteBgBlue" @tap="goPayMoney">
						去支付
					</button>
				</view>
				
			</view>
		</view>
		
	</view> 
</template>

<script>
	import {mapState, mapMutations} from 'vuex'
	
	export default {
		data() {
			return {
				butt:true,
				cart:[]
			}
		},
		props: {
			index:'index'
		},
		computed: {
			...mapState(['orderType', 'address', 'store']),
			numsum(){
				return this.cart.reduce((acc, cur) => acc + cur.number, 0)
			},
			total() {
				return this.cart.reduce((acc, cur) => acc + cur.number * cur.price, 2)
			},
			amount() {
				return this.cart.reduce((acc, cur) => acc + cur.number * cur.price, 0)
			}
		},
		onLoad() {
			this.$store.commit('SET_NAME','柳逸尘')
			console.log(this.$store.state.name)
			this.$store.commit('SET_NAME','柳逸尘222')
			this.cart = uni.getStorageSync('cart')
		},
		methods: {
			addressClick(){
				uni.navigateTo({
					url: './address',
					animationType: 'pop-in',
					animationDuration: 2000
				})
			},
			goPayMoney(){
				uni.switchTab({
					url: '../order/order-list',
					animationType: 'pop-in',
					animationDuration: 2000
				})
			},
			privilegeClick(){
				console.log("--点击优惠说明及图标----1111111------");
			}
		}
	}
</script>

<style>
page{
	background-color: #f7f7f7;
}

.pay{
	background-color: #4f535a;
	width: 90%;
	height: 120rpx;
	margin: 0 auto;
	border-radius: 60rpx;
	
	position: fixed;
	bottom: 30rpx;
	left: 5%;
	z-index: 1000;
}

.go{
	width: 200rpx; 
	height: 90rpx;
	line-height: 90rpx;
	background-color: #676c73 !important;
	color: #cecece !important;
	text-align: center;
	border-radius: 90rpx;
}

.colorWhiteBgBlue{
	background-color: #007AFF !important;
	color: #FFFFFF !important;
}
</style>
