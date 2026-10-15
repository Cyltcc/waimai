<template>
	<view class="content">
		<view class="width95 title">{{title}}</view>
		
		<!-- 轮播图 -->
		<view class="banner-image">
			<!-- <image class="banner-image" src="static/banner.png"></image> -->
			<u-swiper :list="imgList"  keyName="image" mode="dot" border-radius="20" ></u-swiper>
		</view>  
		
		<!-- 搜索框 -->
		<navigator class="width95" url="search">
			<view> 
				<input class="input"  placeholder="输入商品" placeholder-style="color:#b8b8b8" /> 
			</view>
		</navigator>
		
		<!-- 选项 -->
		<view class="width95">
			<view class="width40 d-row d-jc-between">
				<view :class="{active: activeTabIndex == index}"
					v-for="(item,index) in activeList" :key="index" @tap="handleTab(index)">
					<view class="title-name">{{ item.title }}</view>
					<view :class="{titleBottom: activeTabIndex == index}" ></view>
				</view>
			</view>
		</view>

		<view class="width100 shop-list">
			<scroll-view scroll-y="true">
				<view class="width90 d-row d-ai-centen" style="margin: 10rpx auto;height: 180rpx;"
					v-for="(good,index) in shopDetailList" :key=index >
					<view> <image class="shop-list-img" src="/static/coffee.png" /></view>
					<view class="width100 d-row margin-20">
						<view class="width100 d-clo">
							<view class="font-bold font30">{{good.name}}</view>
							<view class="font22 gray3">{{good.type}}</view>
							<view class="font22 gray3">月销{{good.yx}}</view>
							<view class="width100 d-row d-jc-between d-ai-centen">
								<view class="color-red">¥ {{good.price}}</view>
								<view class="d-row">
									<!-- <view v-show="goodCartNum(good.id)" @tap="reducShopClick(good,1)" class="reduc iconfont iconsami-select" ></view> -->
									<view v-show="goodCartNum(good.id)" @tap="reducShopClick(good,1)" class="reduc"> - </view>
									<view v-show="goodCartNum(good.id)" class="number" >{{ goodCartNum(good.id) }}</view>
									<view @tap="addShopClick(good,1)" class="add"> + </view>
								</view>
							</view>
						</view>
					</view>
				</view>
				<view style="height: 100rpx;"></view>
			</scroll-view>
		</view>
		<view class="shop-car">
				<view style="margin:0 auto;" class="width90 d-row d-jc-between d-ai-centen">
					<view class="d-row d-jc-start d-ai-centen" style="height: 120rpx;padding-left: 10rpx;"
						@tap="open"> 
						<view> <image class="shop-car-img" :src="getCartGoodsPrice > 0 ? '/static/cars-blue.png':'/static/cars.png'" ></image> </view>
						<view class="d-clo d-jc-around" style="height: 100rpx;margin-left: 10rpx;"> 
							<view class="gray4 font36" :class="{colorWhite:getCartGoodsPrice > 0}">
								{{ getCartGoodsPrice>0?'￥'+getCartGoodsPrice:shopCarTip}}
							</view>
							<view class="gray4">配送费 <span class="margin-10 font22">¥2</span> | 可自取</view>
						</view>
					</view>
					<view class="go gray4">
						<button class="go gray4 font28" :class="{colorWhiteBgBlue: !disabledPay}" @tap="goPay" :disabled="disabledPay">
							{{ getCartGoodsPrice > 0 ? (disabledPay ? `差${spread}元起送` : goTitle) : goTitle }}
						</button>
					</view>
				</view>
			</view>
			<u-popup :round="10" :show="show" @close="close" @open="open">
				<view class="d-clo d-ai-centen">
					<view class="width90 d-row d-jc-between font28" style="height: 100rpx;line-height: 100rpx;">
						<view>已选商品</view>
						<u-icon  @tap="emptyClick" name="trash" label="清空购物车"></u-icon>
					</view>
					<view class="d-jc-between" style="width: 100%;">
						<view class="width90 d-row d-ai-centen d-jc-between border-bottom-1 padding-10" v-for="(good,index) in cart" :key="index">
							<view class="container">
								<image
									src="/static/coffee.png"
									mode="scaleToFill"
									style="width: 150rpx; height: 150rpx"
								/>
								<view class="info"> 
									<label class="font34 font-bold">{{good.name}}</label>  
									<p class="font26 gray3">{{ good.type }}</p>
									<p class="font34 color-red">￥{{ good.price * good.number }} </p>
								</view>
							</view>
							<view class="d-row padding-left-20">
								<view v-show="goodCartNum(good.id)" @tap="reducShopClick(good,1)" class="reduc">-</view>
								<view v-show="goodCartNum(good.id)" class="number" >{{ goodCartNum(good.id) }}</view>
								<view @tap="addShopClick(good,1)" class="add">+</view>
							</view>
						</view>
					</view>
				</view>
			</u-popup>
	</view>
</template>

<script>
import {mapState} from 'vuex'
	export default {
		data() {
			return {
				show: false,
				title: '中午好,午饭时间到了!',
				number:0,
				cart:[],
				activeList: [
					{title: '口碑最好', value: 'distance'},
					{title: '销量最高', value: 'sales'},
				],
				activeTabIndex: '',
				imgList: [{
						image: '/static/banner.png',
						title: '蒹葭苍苍，白露为霜。所谓伊人，在水一方'
					},
					{
						image: '/static/banner.png',
						title: '蒹葭萋萋，白露未晞。所谓伊人，在水之湄'
					},
					{
						image: '/static/banner.png',
						title: '蒹葭萋萋，白露未晞。所谓伊人，在水之湄'
					},
					{
						image: '/static/banner.png',
						title: '蒹葭萋萋，白露未晞。所谓伊人，在水之湄'
					}
				],
				shopDetailList:[
					{
						"id":'1',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},
					{
						"id":'2',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},{
						"id":'3',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},{
						"id":'4',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},{
						"id":'5',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},
					{
						"id":'6',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},{
						"id":'7',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},
					{
						"id":'8',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					},
					{
						"id":'9',
						"name":'coffee',
						"type":'咖啡',
						"yx":'1200',
						"price":'15'
					}
				],
				shopCarTip:'未选购商品',
				cart:[],
				minPrice: 20
			}
		},
		onLoad() {
			this.activeTabIndex = 0
		},
		methods: {
			handleTab(index) {
				this.activeTabIndex = index
			},
			addShopClick(good,num){
				const index = this.cart.findIndex(item => {
					return item.id === good.id
				})
				if(index > -1) {
					this.cart[index].number += num
				} else {
					this.cart.push({
						id: good.id,
						name: good.name,
						price: good.price,
						number: num,
						type: good.type
					})
				}
			},
			reducShopClick(good){
				const index = this.cart.findIndex(item => item.id === good.id)
				this.cart[index].number -= 1
				if(this.cart[index].number <= 0) {
					this.cart.splice(index, 1)
				}
				if(this.getCartGoodsNumber <= 0){
				}
			},
			goPay(){
				uni.setStorageSync('cart', JSON.parse(JSON.stringify(this.cart)))
				uni.navigateTo({
					url: '../store/pay',
					animationType: 'pop-in',
					animationDuration: 2000
				})
			},
			// goDetail() {
			// 	uni.navigateTo({
			// 		url: '../store/index',
			// 		animationType: 'pop-in',
			// 		animationDuration: 2000
			// 	})
			// },
			open() {
				this.show = true
          		console.log(this.cart);
			},
			close() {
			this.show = false
			// console.log('close');
			},
			emptyClick() {
				this.cart = []
				this.show = false
			}
		},
		computed: {
			...mapState(['orderType', 'address', 'store']),
			goTitle(){
				if(!this.disabledPay) return "去结算"
				return "¥"+this.minPrice+"起送"
			},
			goodCartNum() {	//计算单个商品添加到购物车的数量
				return (id) => this.cart.reduce((acc, cur) => {
					if(cur.id === id) {
						return acc += cur.number
					}
					return acc
				}, 0)
			},
			getCartGoodsNumber() { //计算购物车总数
				return this.cart.reduce((acc, cur) => acc + cur.number, 0)
			},
			getCartGoodsPrice() {	//计算购物车总价
				return this.cart.reduce((acc, cur) => acc + cur.number * cur.price, 0)
			},
			disabledPay() { //是否达到起送价
				return ((this.getCartGoodsPrice < this.minPrice) ? true : false)
			},
			spread() { //差多少元起送
				return parseFloat((this.minPrice - this.getCartGoodsPrice).toFixed(2))
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
	}
	.title{
		font-size: 30rpx;
		font-weight: bold;
		margin-top: 10rpx;
	}
	.banner-image{
		margin-top: 20rpx;
		margin-bottom: 20rpx;
		width: 95%;
		border-radius:0 0 50rpx 50rpx;
	}
	.input{
		background-color: #f7f7f7;
		border-radius: 10rpx;
		padding: 20rpx;
	}
	
	.title-name{
		padding: 15rpx 0;
	}
	.active {
		font-weight: bold;
		color: #007AFF;
	}
	.titleBottom{
		margin: 0 auto;
		width: 60%;
		border-bottom: 5rpx solid #007AFF;
	}
	
	.s-head{
		flex: 1;
	}
	.s-head-img{
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
	}
	.s-detail{
		flex: 3;
	}
	
	.s-head-img{
		width: 120rpx;
		height: 120rpx;
		border-radius: 50%;
	}
	
	.title-name{
		padding: 15rpx 0;
	}
	.active {
		font-weight: bold;
	}
	.titleBottom{
		margin: 0 auto;
		width: 100%;
		border-bottom: 5rpx solid #007AFF;
	}
	
	.pram{
		border-bottom: 1rpx solid #f5f5f5;
		height: 90rpx;
		line-height: 90rpx;
	}
	.phone-tag{
		color: #007AFF;
		border: 1rpx solid #007AFF;
		border-radius: 50%;
		padding: 3rpx;
	}
	.button-bottom{
		height: 80rpx;
		background-color: #FFFFFF;
		text-align: center;
		line-height: 80rpx;
		border-radius: 40rpx;
	}
	.shop-bar{
		background-color: #f7f7f7;
		flex: 1;
		height: 800rpx;
	}
	.shop-bar-title{
		padding-top: 20rpx;
		height: 90rpx;
		line-height: 70rpx;
		text-align: center;
	}
	.activecolor{
		background-color: #FFFFFF;
	}
	.shop-list{
		flex: 3;
		margin-top: 20rpx;
		position: relative;
	}
	.shop-list-img{
		width: 150rpx;
		height: 150rpx;
	}
	.shop-car{
		background-color: rgba(79, 83, 90, 0.8);
		width: 90%;
		height: 120rpx;
		margin: 0 auto;
		border-radius: 60rpx;
		position: fixed;
		bottom: 100rpx;
		left: 5%;
		z-index: 1000;
	}
	.shop-car-img{
		width: 130rpx;
		height: 130rpx;
		position: relative;
		top: -10rpx;
	}
	
	.reduc{
		color: #007AFF;
		background-color: #FFFFFF;
		width: 40rpx;
		height: 40rpx;
		border-radius: 50%;
		text-align: center;
		line-height: 40rpx;
		border: 1rpx solid #007AFF;
	}
	.number {
		width: 40rpx;
		height: 40rpx;
		line-height: 40rpx;
		text-align: center;
	}
	.add{
		color: #FFFFFF;
		background-color: #007AFF;
		width: 40rpx;
		height: 40rpx;
		border-radius: 50%;
		text-align: center;
		line-height: 40rpx;
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
	
	.click-show{
		width: 100rpx;
		height: 80rpx;
		text-align: center;
		line-height: 80rpx;
		border-radius: 10rpx;
	}
	.click-show:hover{            
		/*鼠标悬浮效果*/
		/*阴影*/
		/* box-shadow: 5px 5px 5px grey; */
		/*发光*/
		background-color: #c6c6c6;
		
	}
	.cart-icon {
		position: absolute;
		right: 10%;
		bottom: 10%;
	}

	.container {
		display: flex;
		align-items: center;
	}

	.image {
		width: 150rpx;
		height: 150rpx;
		margin-right: 20rpx; /* 可选：增加图片和文本之间的间距 */
	}

	.info {
		display: flex;
		flex-direction: column;
	}
</style>
