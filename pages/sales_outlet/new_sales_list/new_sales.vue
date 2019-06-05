<template>
  <view class>
    <view class="billingInfo">
      <view class="thead">
        <view class="theadOne"></view>
        <view class="theadTwo">购货金额</view>
        <view class="theadThree">数量</view>
      </view>
      <view class="item" v-for="item in goodsList" :key="item.id">
        <view class="one">
          <view>{{ item.goodsName }}</view>
          <view>{{ item.goodsCode }}</view>
        </view>
        <view class="two">¥{{ item.money }}</view>
        <view class="three">{{ item.sum }}</view>
      </view>
      <view class="total">合计：19.0</view>
    </view>
    <view class="confirmation">
      <view class="title">结算信息确认</view>
      <view class="">
        <text class="left">本次付款</text>
				<input class="uni-input"  placeholder-style="color:#CCCCCC"  name="" value="19.0" placeholder="19.0" />
      </view>
      <view class="">
        <text class="left">结算账户</text>
        <text class>默认账户</text>
      </view>
      <view class="">
        <text class="left">本次欠款</text>
        <text class>0</text>
      </view>
			<view class="uni-btn-v">
        <button class="" @click="saveAdd">保存并新增</button>
			</view>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      goodsList: [
        {
          goodsName: "高效氯氟氰菊酯",
          goodsCode: "PD20151288",
          money: "13.0",
          sum: 1
        },
        {
          goodsName: "噻虫嗪",
          goodsCode: "PD20140414",
          money: "6.0",
          sum: 2
        }
      ]
    };
  },
  methods: {
    saveAdd(){

    }
  },
};
</script>

<style scoped>
page {
  background: #f2f2f2;
  position: relative;
  height: 100%;
  padding-top: 10upx;
}
.billingInfo {
  background-color: #fff;
}
.thead {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  font-size: 28upx;
  text-align: center;
  padding: 20upx 0;
}
.theadOne{
  padding: 20upx 0 20upx 20upx;
}
.item {
  display: flex;
  align-items: center;
  font-size: 35upx;
  text-align: center;
  padding: 20upx 0 20upx 20upx;
  border-bottom: #f0f0f0 1px solid;
}
.theadOne,
.one {
  flex: 1.5;
  text-align: left;
}
.theadTwo,
.two {
  flex: 1;
}
.two{
  font-weight: 600;
  color: #009688;
}
.theadThree,
.three {
  flex: 1;
}
.three {
  font-size: 26upx;
  color: #ffa31c;
}
.total {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  height: 100upx;
  padding-right: 100upx;
  font-size: 30upx;
  font-weight: 600;
  color: #136cae;
}
.confirmation{
  background-color: #fff;
  position: absolute;
  bottom: 0;
  width: 100%;
  font-size: 30upx;
}
.confirmation>view{
  border-bottom: 1px solid #DDDDDD;
  display: flex;
  align-items: center;
  height: 84upx;
}
.confirmation .title{
  justify-content: center;
  font-weight: 600;
}
.confirmation .uni-btn-v{
  height: 120upx;
  padding-right: 40upx;
  justify-content: flex-end;
}
.confirmation .left{
  width: 140px;
  text-align: center;
}
.uni-btn-v button{
  background-color: #136CAE;
  color: #fff;
  height: 60upx;
  line-height: 60upx;
  width: 280upx;
  font-size: 30upx;
  margin: 0;
}
</style>
