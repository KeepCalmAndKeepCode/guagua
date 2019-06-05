<template>
  <view class="content">
    <view class="main">
      <view class @click="skipNewSupplier">
        <text class="iconfont iconxinzeng"></text>
        <view class="mainContent">
          <view>新增购货单</view>
        </view>
      </view>
      <view class="" @click="skipQueryOrder">
        <text class="iconfont iconchaxun"></text>
        <view class="mainContent">
          <view>查询购货单</view>
        </view>
      </view>
      <view class>
        <text class="iconfont iconxinzeng"></text>
        <view class="mainContent">
          <view>新增购货退货单</view>
        </view>
      </view>
      <view class>
        <text class="iconfont iconchaxun"></text>
        <view class="mainContent">
          <view>查询购货退货单</view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
export default {
  methods: {
    skipNewSupplier(){
      uni.navigateTo({
        url: "/pages/procurementStorage/purchase/newSupplier"
      });
    },
    skipQueryOrder(){
      uni.navigateTo({
        url: "/pages/procurementStorage/queryOrder"
      });
    },
  },
}
</script>

<style scope>
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
.main > view .iconfont {
  font-size: 100upx;
  color: #136cae;
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

</style>
