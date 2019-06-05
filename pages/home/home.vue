<template>
  <view class="content">
    <view class="main">
      <view class @click="skipCommodityManage">
        <text class="iconfont iconhuoche"></text>
        <view class="mainContent">
          <view>商品管理</view>
          <view>查阅及新增商品</view>
        </view>
      </view>
      <view class="" @click="skipProcurementStorage">
        <text class="iconfont iconccgl-rukuguanli-1"></text>
        <view class="mainContent">
          <view>采购入库</view>
          <view>处理进货及购货退货</view>
        </view>
      </view>
      <view class="" @click="skipSalesOutlet">
        <text class="iconfont iconchuku"></text>
        <view class="mainContent">
          <view>销货出库</view>
          <view>处理销贷及销货退货</view>
        </view>
      </view>
      <view class="" @click="skipMaterial">
        <text class="iconfont iconziliaoguanli"></text>
        <view class="mainContent">
          <view>资料管理</view>
          <view>管理客户 供应商 仓库</view>
        </view>
      </view>
    </view>
    <view class="banner">
      <view>
        <view class="left">
					<text>贯彻落实</text>
					<text class="two">国家法律</text>
				</view>
				<view class="right">
					贯彻落实《农药管理条例》，对农资经营者实行台账管理
				</view>
      </view>
      <image src="../../static/homeimg.png" mode=""></image>
    </view>
  </view>
</template>
<script>
export default {
  data() {
    return {
      title: "Hello world"
    };
  },
  
  methods: {
    skipCommodityManage(){
      uni.navigateTo({
        url:"/pages/commodityManage/commodityManage"
      });
    },
    skipProcurementStorage(){
      uni.navigateTo({
        url:"/pages/procurementStorage/procurementStorage"
      });
    },
    skipProcurementStorage(){
      uni.navigateTo({
        url:"/pages/procurementStorage/procurementStorage"
      });
    },
    skipSalesOutlet(){
      uni.navigateTo({
        url:"/pages/sales_outlet/sales_outlet"
      });
    },
    skipMaterial(){
      uni.navigateTo({
        url:"/pages/data_management/material"
      });
    }
  },
	onNavigationBarButtonTap(e) {
	  uni.navigateTo({
	    url: "/pages/home/set"
	  });
	},
}
</script>
<style scoped>
page {
  background-color: #efefef;
}
.main {
  background: #fff;
}
.main > view {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 200upx;
  border-bottom: 2px solid #efefef;
}
.mainContent {
  padding-left: 40upx;
  width: 300upx;
}
.mainContent :nth-child(1) {
  font-size: 40upx;
  color: #136cae;
  font-weight: 600;
}
.mainContent :nth-child(2) {
  font-size: 30upx;
  color: #9f9f9f;
}
.iconfont{
	color: #136cae;
	font-size: 90upx;
}
.banner {
  background: #fff;
  display: flex;
  align-items: center;
  height: 280upx;
  margin-top: 10upx;
}
.banner image {
  width: 174upx;
  height: 174upx;
  border-radius: 50%;
  margin-right: 80upx;
}
.banner > view {
  flex: 1;
  padding: 0 50upx;
}
.banner > view > .left{
  font-weight: 600;
  text-align: center;
  padding-bottom: 20upx;
}
.banner > view > .left .two{
  margin-left: 20upx;
  color: #ff9800;
}
.banner > view > .right{
  font-size: 25upx;
  color: #a0a0a0;
}
</style>