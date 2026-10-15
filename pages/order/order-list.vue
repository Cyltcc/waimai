<template>
	<view class="width95 margin-center ">
		<template>
		    <u-tabs @click="handleClick" :list="list1"></u-tabs>
		</template>
		
		<view v-for="order in orders" :key="order.id" v-show="shouldShowList(order.status)" class="bg-color-white padding-20 border-radius-15 margin-top-30">
		      <view @click="handleDedail" class="d-row d-jc-between padding-10 padding-bottom-20 border-bottom-2">
		        <view class="d-row">
		          <view class="d-clo d-ai-centen">
		            <image style="width: 80rpx; height: 80rpx;" :src="order.image"></image>
		          </view>
		          <view class="d-clo d-jc-between padding-left-10">
		            <h3>{{ order.name }}</h3>
		            <label class="font20 gray3">{{ order.time }}</label>
		          </view>
		        </view>
		        <view class="d-clo d-ai-end">
		          <view class="padding-bottom-20">{{ order.statusText }}</view>
		          <view>
		            <label class="font24 gray3">{{ order.statusDescription }}</label>
		          </view>
		        </view>
		      </view>
		      <view class="d-row d-jc-between d-ai-centen padding-10 font30 padding-bottom-10">
		        <view class="font22">{{ order.name }}</view>
		        <view>¥ <label class="font-bold font36">{{ order.price }}</label></view>
		      </view>
		      <view v-if="order.status === '待支付'" class="d-row d-jc-between d-ai-centen padding-10 padding-top-30">
		        <view></view>
		        <view>
		          <button plain type="primary" size="mini" @tap="go('pay')">去支付</button>
		        </view>
		      </view>
		      <view v-if="order.status === '已取消'" class="d-row d-jc-between d-ai-centen padding-10 padding-top-30">
		        <view></view>
		        <view>
		          <button plain type="primary" size="mini" @tap="go('one')">再来一单</button>
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
				  list1: [
					  {
						  name: '全部'
					  },
					  {
						  name: '待支付'
					  },
					  {
						  name: '已完成'
					  }
				  ],
				  orders: [],
				  activeTabIndex: 0
			}
		},
		methods: {
			go(type){
				if('one'==type){
					console.log("one--------")
					uni.switchTab({
						url: '/pages/index/index'
					});
				}else if('pay'==type){
					console.log("pay--------")
					uni.navigateTo({
						url: '../store/pay',
						animationType: 'pop-in',
						animationDuration: 2000
					})
				}
			},
			handleClick(item) {
				console.log(item)
				this.activeTabIndex = item.index
			},
			handleDedail() {
				uni.navigateTo({
					url:'./order-dedail'
				})
			},
			 shouldShowList(status) {
			      return this.activeTabIndex === 0 || this.activeTabName === status;
			    },
				 fetchOrders() {
				      // 模拟请求获取数据
				      this.orders = [
				        { id: 1, name: 'coffee', image: '../../static/coffee.png', time: '15小时 2分钟前', status: '待支付', statusText: '等待支付', statusDescription: '剩余 09:15', price: 15 },
				        { id: 2, name: 'coffee', image: '../../static/coffee.png', time: '15小时 2分钟前', status: '已取消', statusText: '已取消', statusDescription: '订单超时未支付', price: 15 },
				        { id: 3, name: 'coffee', image: '../../static/coffee.png', time: '15小时 2分钟前', status: '已完成', statusText: '已送达', statusDescription: '', price: 15 }
				      ];
				    }
		},
		  computed: {
		    activeTabName() {
		      return this.list1[this.activeTabIndex].name;
		    }
		  },
		  mounted() {
		      this.fetchOrders();
		    }
	}
</script>

<style>
uni-page-head,uni-page,page{
	background-color: #f7f7f7;
}

.bgColor{
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	bottom: 0;
	background: #bbbbf3;
	z-index: -1;
}
</style>
