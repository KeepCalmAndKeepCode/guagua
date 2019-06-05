<template>
  <view class>
    <view class="query">
      <view class="search">
        <text class="iconfont iconfangdajing"></text>
        <input
          class="uni-input"
          placeholder-style="color:#888888"
          name="keyword"
          placeholder="键入关键字查询"
        >
      </view>
      <view class="inputDate">
        <view class>
          <picker class="datePicker two" mode="date" :value="date" @change="bindDateChange">
            <view class="uni-input">{{date}}</view>
          </picker>
          <text class="iconfont iconrili three"></text>
        </view>
        到
        <view class>
          <picker class="datePicker two" mode="date" :value="date1" @change="bindDateChange1">
            <view class="uni-input">{{date1}}</view>
          </picker>
          <text class="iconfont iconrili three"></text>
        </view>
      </view>
    </view>
    <view class="content">
      <view class="item1">
        <text class="">供应商：河南龙之云有限责任公司</text>
        <text class="price">¥19.0</text>
      </view>
      <view class="item2">单据编号：CG201809041037273</view>
      <view class="item3">
        <text class="item3one">2018-09-04</text>
        <text class="">共2件商品</text>
        <text class="item3-3">全部付款</text>
      </view>
      <view class="item4">
        <view class="" @click="togglePopup('bottom-share')">
        <text class="fontColor">打印二维码</text>
        <text class="iconfont icondayinjidayinxianxing"></text>
        </view>
        <view class="">
        <text class="fontColor">打印小票</text>
        <text class="iconfont icondayinjidayinxianxing"></text>
        </view>
      </view>
      <view class="item5">查看详细</view>
    </view>
    <uni-popup :show="type === 'bottom-share'" position="bottom" mode="bottom" @hidePopup="togglePopup('')">
      <view class="">{{ productName }}</view>
      <image class="qrcode" src="../../static/qrcode.png"></image>
      <view class="uni-center center-box print">
        打印
      </view>
      <view class="uni-center center-box" @click="togglePopup('')">
        取消
      </view>
		</uni-popup>
  </view>
</template>

<script>
import uniPopup from '../components/uni-popup';
export default {
  components: {
    uniPopup
  },
  data() {
    return {
      type: '',
      productName:"",
      date: this.getDate(),
      date1: this.getDate()
    };
  },
  computed: {
    startDate() {
      return this.getDate("start");
    },
    endDate() {
      return this.getDate("end");
    }
  },
  methods: {
    bindDateChange(e){
      this.date = e.target.value;
    },
    bindDateChange1(e){
      this.date1 = e.target.value;
    },
    togglePopup(type) {
      this.type = type;
    },
    getDate(type) {
      const date = new Date();
      let year = date.getFullYear();
      let month = date.getMonth() + 1;
      let day = date.getDate();

      if (type === "start") {
        year = year - 60;
      } else if (type === "end") {
        year = year + 2;
      }
      month = month > 9 ? month : "0" + month;
      day = day > 9 ? day : "0" + day;
      return `${year}-${month}-${day}`;
    },
  },
};
</script>

<style scoped>
page {
  padding-top: 10upx;
  background: #f2f2f2;
}
.search {
  position: relative;
  padding: 30upx;
}
.search .uni-input {
  background: #f2f2f2;
  font-size: 30upx;
  height: 81upx;
  line-height: 81upx;
  padding-left: 80upx;
  border-radius: 50upx;
}
.search .iconfangdajing {
  position: absolute;
  z-index: 10;
  top: 50%;
  transform: translate(30upx, -50%);
}
.query {
  background-color: #fff;
  padding-bottom: 20upx;
}

.inputDate{
  display: flex;
  align-items: center;
}
.inputDate > view {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}
.inputDate .iconrili{
  margin-left: 20upx;
}

.content{
  background-color: #fff;
  margin-top: 10upx;
}

.content>view{
  display: flex;
  align-items: center;
  height: 70upx;
  padding: 0 40upx;
}
.item1{
  font-size: 32upx;
}
.item1>.price{
  flex: 1;
  text-align: right;
  font-size: 20px;
  color: #0F9C8F;
}
.item2{
  font-size: 26upx;
  color: #666666;
}
.item3{
  font-size: 26upx;
}
.item3one{
  flex: 1;
}
.item3-3{
  color: #EB7605;
  margin-left: 30upx;
}
.item4{
  font-size: 26upx;
  border-bottom: 1px solid #F0F0F0;
  justify-content: flex-end;
}
.item4 .iconfont{
  margin: 0 10upx;
}
.item4 .fontColor{
  color: #136CAE;
}
.item5{
  font-size: 30upx;
  justify-content: center;
  color: #136CAE;
}
.print{
  color: #2c84cb;
  border-top: #BBBBBB 1px solid;
  border-bottom: #BBBBBB 1px solid;
}
.qrcode{
  width: 510upx;
  height: 510upx;
}
</style>
