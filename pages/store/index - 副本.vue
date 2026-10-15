<template>
	<view class="">
		
		<!-- 商家信息 -->
		<view class="width100 d-clo d-jc-centen d-ai-centen">
			<view class="margin-top-30"> <image class="s-head-img" :src="form.headImg"></image> </view>
			<view class="font-bold font38 gray1 margin-top-30">{{form.name}}</view>
			<view class="gray2 margin-top-30">月售 780单 | 专送约40分钟</view>
			<view class="gray4 margin-top-30">公告:疫情期间送达可能会延长,请耐心等待</view>
		</view>
		
		<view style="position: relative;bottom: 200rpx;left: 660rpx;" @tap="collectionClick">
			<u-icon v-if="!collection" name="heart"  size="36" color="#2979ff" ></u-icon>
			<u-icon v-else name="heart-fill"  size="36" color="#2979ff" ></u-icon>
		</view>
		
		<!-- 选项 -->
		<view class="content width100">
			<view class="width60 d-row d-ai-centen d-jc-between">
				<view :class="{active: activeTabIndex == index}"
					v-for="(item,index) in activeList" :key="index" @tap="handleTab(index)">
					<view class="title-name font32">{{ item.title }}</view>
					<view :class="{titleBottom: activeTabIndex == index}" ></view>
				</view>
			</view>
		</view>
		
		<view  style="margin-top: 20rpx;border-bottom: 1rpx solid #e2e2e2;"></view>
		
		<!-- 商品 -->
		<view v-if="activeTabIndex==0" class="shop">
			<view class="d-row">
				<!-- 商品导航 -->
				<view class="shop-bar">
					<view class="shop-bar-title " :class="{activecolor:shopBarTitleIndex == index}"
						v-for="(item,index) in shopBarTitleList" :key="index" @tap="shopBarTitleClick(index)">
						{{ item.title }}
					</view>
				</view>
				
				<!-- 商品列表 -->
				<view class="width100 shop-list">
					<scroll-view scroll-y="true" style="height: 700rpx;">
						<view class="font-bold" style="margin-top: 20rpx; margin-left: 20rpx;">{{shopDetailListTile}}</view>
						
						<view class="width85 d-row d-ai-centen" style="margin: 10rpx auto;height: 180rpx;"
							v-for="(item,index) in shopDetailList" :key=index >
							<view> <image class="shop-list-img" src="/static/logo.png"></image></view>
							<view class="width100 d-row margin-20">
								<view class="width100 d-clo">
									<view class="font-bold font30">{{item.name}}</view>
									<view class="font22 gray3">{{item.type}}</view>
									<view class="font22 gray3">月销{{item.yx}}</view>
									<view class="width100 d-row d-jc-between d-ai-centen">
										<view class="color-red">¥ {{item.price}}</view>
										<view class="add" @tap="addShopClick(index)">
											<view v-if="addShopTF">+</view>
											<view v-if="!addShopTF" style="position: relative;left: -120rpx;">
												<u-number-box
												:index="index"
												:value="1" 
												@change="valChange"
												bg-color="#007AFF"
												color="#FFFFFF"
												size="10"
												input-width="40"
												input-height="30"
												></u-number-box>
											</view>
										</view>
									</view>
								</view>
							</view>
						</view>
						<view style="height: 120rpx;"></view>
					</scroll-view>
				</view>
				
			</view>
			
			<view class="shop-car">
				<view style="margin:0 auto;" class="width90 d-row d-jc-between d-ai-centen">
					<view class="d-row d-jc-start d-ai-centen" style="height: 120rpx;padding-left: 40rpx;"> 
						<view> <image class="shop-car-img" src="/static/logo.png"></image> </view>
						<view class="d-clo d-jc-around" style="height: 100rpx;margin-left: 10rpx;"> 
							<view class="gray4 font30">{{shopCarTip}}</view>
							<view class="gray4">配送费 <span class="margin-10 font16">¥2</span> | 可自取</view>
						</view>
					</view>
					<view class="go gray4">
						¥10起送
					</view>
				</view>
			</view>
		</view>
		
		<!-- 商家详情 -->
		<view v-if="activeTabIndex==1" class="content" >
			<view class="width90 d-clo d-jc-around" style="height: 360rpx;">
				<view>商家信息</view>
				<view><image style="width: 200rpx;height: 200rpx;" src="../../static/logo.png"></image></view>
				<view>{{form.name}}</view>
			</view>
			
			<view class="width100" style="background-color: #F7F7F7;height: 20rpx;"></view>
			
			<view class="width90 d-row d-jc-between pram" >
				<view class="font-bold font34">商家名称</view>
				<view class="gray2 font30">{{form.name}}</view>
			</view>
			<view class="width90 d-row d-jc-between pram" >
				<view class="font-bold font34">商家品类</view>
				<view class="gray2 font30">{{form.type}}</view>
			</view>
			<view class="width90 d-row d-jc-between pram" >
				<view class="font-bold font34">商家地址</view>
				<view class="gray2 font30">{{form.address}}</view>
			</view>
			<view class="width90 d-row d-jc-between pram" >
				<view class="font-bold font34">商家电话</view>
				<view class="gray2 font30">
					<u-icon name="phone-fill" class="phone-tag"></u-icon>
					{{form.phone}}
				</view>
			</view>
			<view class="width90 d-row d-jc-between pram" style="border-bottom: 0;" >
				<view class="font-bold font34">营业时间</view>
				<view class="gray2 font30">{{form.time}}</view>
			</view>
			
			<view @tap="toReportClick" class="width100 d-clo d-jc-centen d-ai-centen" style="background-color: #F7F7F7;height: 150rpx;">
				<view class="width90 button-bottom font-bold font34">举报商家</view>
			</view>
		</view>
		
		<!-- 提示框 -->
		<u-toast ref="uToast" />
	</view>
</template>

<script>
	export default {
		data() {
			return {
				collection:false,
				addShopTF:true,
				form:{
					id:'',
					name:'',
					headImg:'',
					phone:'18637815946',
					time:'10:00~22:00',
					address:'深证市罗湖区翠竹路1052号102室',
					type:'水果生鲜'
				},
				activeList: [
					{title: '点餐', value: 'd'},
					{title: '商家', value: 's'},
				],
				shopBarTitleList:[
					{title:'招牌',value:'a'},
					{title:'热销',value:'b'},
					{title:'爆品',value:''},
				],
				activeTabIndex: '',
				shopBarTitleIndex:'',
				show:true,
				shopDetailListTile:'',
				shopDetailList:[
					{
						"id":'1',
						"name":'草莓',
						"type":'水果',
						"yx":'1200',
						"price":'20'
					},
					{
						"id":'2',
						"name":'苹果',
						"type":'水果',
						"yx":'1200',
						"price":'20'
					},
					{
						"id":'3',
						"name":'香蕉',
						"type":'水果',
						"yx":'1200',
						"price":'22'
					},
					{
						"id":'4',
						"name":'蓝莓',
						"type":'水果',
						"yx":'1200',
						"price":'20'
					},
					{
						"id":'5',
						"name":'西瓜',
						"type":'水果',
						"yx":'12100',
						"price":'21'
					}
				],
				shopCarTip:'未选购商品'
			}
		},
		onLoad(option) {
			this.activeTabIndex=0;
			this.shopBarTitleIndex=0;
			
			console.log(JSON.stringify(option) +"详情页面接收--"+option.id); //打印出上个页面传递的参数。
			this.form.id = option.id;
			this.form.name = option.name;
			this.form.headImg = option.headImg;
		},
		onNavigationBarSearchInputClicked(){
			uni.navigateTo({
				url:'../index/search',
				animationType: 'pop-in',
				animationDuration: 2000
			})
		},
		watch: {
			// watch 监听器,监听到数据发生变化时执行
			activeTabIndex: async function(val) {
				const type = this.activeList[val].value
				await this.getForm(type)
			},
			shopBarTitleIndex: async function(val){
				this.shopDetailListTile = this.shopBarTitleList[val].title
				const type = this.shopBarTitleList[val].value
				await this.getShopBarTitleList(type)
			}
		},
		methods: {
			handleTab(index) {
				this.activeTabIndex = index
			},
			shopBarTitleClick(index){
				this.shopBarTitleIndex = index
				this.shopDetailListTile = this.shopBarTitleList[index].title
			},
			async getForm(type){
				console.log("获取"+type+"表单")
			},
			async getShopBarTitleList(type){
				console.log("获取"+type+"商品种类")
			},
			collectionClick(){
				this.collection = !this.collection;
				if(this.collection){
					this.$refs.uToast.show({
						title: '收藏成功',
						type: 'success',
						position:'top',
						duration:'500'
					})
				}else{
					this.$refs.uToast.show({
						title: '取消收藏',
						type: 'warning',
						position:'top',
						duration:'500'
					})
				}
			},
			toReportClick(){
				this.$refs.uToast.show({
					title: '举报成功',
					type: 'success',
					position:'center',
					duration:'500'
				})
			},
			valChange(index){
				console.log(JSON.stringify(index)+"-----------"+index)
				
			},
			addShopClick(index){
				console.log("点击第"+index+"个")
				this.addShopTF = false;
			}
		}
	}
</script>

<style>
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
	
	.shop{
		
	}
	.shop-bar{
		background-color: #f7f7f7;
		flex: 1;
		height: 600rpx;
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

	}
	.shop-list-img{
		width: 150rpx;
		height: 150rpx;
	}
	.shop-car{
		background-color: #4f535a;
		width: 90%;
		height: 120rpx;
		margin: 0 auto;
		border-radius: 60rpx;
		
		position: fixed;
		bottom: 20rpx;
		left: 5%;
	}
	.shop-car-img{
		width: 80rpx;
		height: 130rpx;
		
		position: relative;
		top: -10rpx;
	}
	
	.add{
		color: #FFFFFF;
		background-color: #007AFF;
		width: 40rpx;
		height: 40rpx;
		border-radius: 50%;
		text-align: center;
		line-height: 36rpx;
	}
	.go{
		width: 160rpx; 
		height: 90rpx;
		line-height: 90rpx;
		background-color: #676c73;
		text-align: center;
		border-radius: 90rpx;
	}
</style>
