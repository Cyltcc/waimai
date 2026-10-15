<template>
  <view class="container">
    <view class="map-container" id="mapContainer"></view>
    <view @touchstart="handleTouchStart" @touchmove="handleTouchMove" :style="{ height: containerHeight + 'px' }" class="info-container">
      <view class="delivery-man">
        <view class="delivery-info">
          <text class="name">XXX</text>
          <view class="rating">
            <text>★★★★★</text>
          </view>
        </view>
      </view>
      <scroll-view class="delivery-details" scroll-y="true" style="height: 500px;">
        <text class="title">xxxx</text>
        <text class="address">收货地址: xxx</text>
        <view class="status">
          <view class="status-icon">
            <text class="status-text">已完成</text>
          </view>
          <text class="status-time">
            您的订单已签收。<br />如有疑问可以联系配送员确认。
            <br />感谢您的购物，欢迎再次光临。
          </text>
          <text class="timestamp">2024-07-06 10:03:32</text>
        </view>
        <u-steps direction="column" :current="5">
          <u-steps-item v-for="item in steps" :title="item.title" :desc="item.content"></u-steps-item>
        </u-steps>
      </scroll-view>
    </view>
  </view>
</template>


<script>
import plane from '@/static/wurenji.png'
import startImg from '@/static/start.png'
import endImg from '@/static/end.png'
import AMapLoader from '@amap/amap-jsapi-loader'

export default {
  data() {
    return {
      isAtMaxHeight: false, // 记录是否已经达到最大高度
      currentLocationInfo: {latitude:"31.93182", longitude:"118.633415"},
      containerHeight: window.innerHeight * 0.33, // 初始高度为屏幕高度的1/3
      map: null,
      logisticsInfoList: [
          {latitude: '30.454012', longitude: '114.42659'},
          {latitude: '31.93182', longitude: '118.633415'},
          {latitude: '31.035032', longitude: '121.611504'}
        ],
	     activeStep: 4,
	        steps: [
	          {
	            title: '运输中',
	            content: '您的订单已到达XXXX'
	          },
	          {
	            title: '运输中',
	            content: '您的订单在XXXX收货完成  2024-07-06 08:38:22',
	          },
	          {
	            title: '运输中',
	            content: '您的订单已离开XXX，前往XXXX中  2024-07-06 08:38:22'
	          },
	          {
	            title: '运输中',
	            content: '您的订单XXXX  2024-07-06 08:38:22'
	          },
	          {
	            title: '派送中',
	            content: '您的订单正在配送途中（快递员：xxx，电话：xxxxxxxxxxx），请您耐心等待。 2024-07-06 08:38:22 '
	          },
	          {
	            title: '已签收',
	            content: '您的订单签收。感谢您的购物，欢迎再次光临。 2024-07-06 08:38:22 '
	          }
	        ],
          initialHeight: 0,
          zoomLevel: 14, // 当前地图缩放级别
	  };
  },
  mounted() {
    window._AMapSecurityConfig = {
		    securityJsCode: "b9ef74bfdc76553e32f9199dbcf71215"
		  };
      this.initMap()
  },
  methods: {
    initMap() {
        AMapLoader.load({
          key: '49ba12616a0e92197219607460d6fe21', // 申请好的Web端开发者Key，首次调用 load 时必填
          version: '2.0' // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        }).then((AMap) => {
          // 初始化高德地图
          this.map = new AMap.Map('mapContainer', {
            zoom: 7,
            mapStyle: "amap://styles/whitesmoke"
          });

          // 使用箭头函数以保持 `this` 上下文
          AMap.plugin('AMap.Driving', () => {
            const driving = new AMap.Driving({
              map: this.map, // 使用 this.map 而不是 map
              showTraffic: false,
              hideMarkers: true
            });

            const getMarker = (point, image, width, height) => {
              const icon = new AMap.Icon({
                size: new AMap.Size(width, height),
                image,
                imageSize: new AMap.Size(width, height)
              });
              const marker = new AMap.Marker({
                position: point,
                icon: icon,
                offset: new AMap.Pixel(-width / 2, -height) // AMap.Pixel 像素坐标，确定地图上的一个像素点。
              });
              return marker;
            };

            let startLngLat = [117.2147, 39.11712]; // 起始的经纬度
            // 使用标点
            const startMarker = getMarker(startLngLat, startImg, 30, 30);
            this.map.add(startMarker);

            let endLngLat = [117.2197, 39.11742]; // 结束的经纬度
            // 使用标点
            const endMarker = getMarker(endLngLat, endImg, 30, 30);
            this.map.add(endMarker);

            driving.search(startLngLat, endLngLat, {
              // waypoints: [
              //   [116.23128, 40.22077], // 北京
              //   [119.48458, 39.83507], // 秦皇岛
              //   [121.5255, 38.95223], // 大连
              //   [123.46987, 41.80515], // 沈阳
              //   [125.28845, 43.83327] // 长春
              // ]
            }, (status, result) => { 
                const curr = [117.2197, 39.11742];
                const currMarker = getMarker(curr, plane, 30, 30);
                this.map.add(currMarker);
                setTimeout(() => {
                  this.map.setFitView(
                    [currMarker] // 覆盖物数组
                  );
                  this.map.setZoom(15);
                }, 3000);
            });
          });
        }).catch(e => {
        console.error(e);
      });
    },
    handleTouchStart(e) {
      this.startY = e.touches[0].pageY;
    },
    handleTouchMove(e) {
      const moveY = e.touches[0].pageY;
      const deltaY = moveY - this.startY;

      if (deltaY < 0) {
        // 向上滑动，固定高度到最大值
        this.containerHeight = 600;
      }  else {
        // 向下滑动，固定高度到最小值
        this.containerHeight = 280;
      }
      
      this.startY = moveY;
    },
  },
};
</script>

<style scoped>
.container {
  position: relative;
  width: 100%;
  height: calc(100vh - 44px - env(safe-area-inset-top));
  overflow: hidden;
}

.map-container {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 66.7vh;
  z-index: 1;
}

.info-container {
  width: 89.7vw;
  position: absolute;
  left: 50%;
  bottom: 0;
  transform: translateX(-50%);
  overflow: hidden;
  z-index: 999;
  font-size: 20px;
  line-height: 30px;
  background-color: #fff;
  padding: 20rpx;
  border-radius: 16rpx;
  box-shadow: 0 4rpx 8rpx rgba(0, 0, 0, 0.1);
  transition: height 0.3s ease, top 0.3s ease;
}

.delivery-man {
  display: flex;
  align-items: center;
  margin-bottom: 20rpx;
  font-size: 20px;
}

.avatar {
  width: 100rpx;
  height: 100rpx;
  border-radius: 50%;
  margin-right: 20rpx;
}

.delivery-info {
  display: flex;
  flex-direction: column;
}

.name {
  font-size: 28rpx;
  font-weight: bold;
  margin-bottom: 4rpx;
}

.rating text {
  font-size: 24rpx;
  color: gold;
}

.delivery-details {
  border-top: 1px solid #f0f0f0;
  padding-top: 20rpx;
}

.title {
  font-weight: bold;
  font-size: 28rpx;
  margin-bottom: 10rpx;
}

.address {
  font-size: 24rpx;
  color: #666;
  margin-bottom: 20rpx;
}

.status {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.status-icon {
  background-color: #e95454;
  color: #fff;
  padding: 10rpx;
  border-radius: 50%;
  margin-bottom: 10rpx;
}

.status-text {
  font-size: 24rpx;
  font-weight: bold;
}

.status-time {
  font-size: 24rpx;
  color: #666;
  line-height: 1.5;
}

.timestamp {
  font-size: 24rpx;
  color: #999;
  margin-top: 10rpx;
}
</style>
