<template>
  <view class>
    <view class="billingInfo">
      <view class="thead">
        <view class="theadOne"></view>
        <view class="theadTwo">购货金额</view>
        <view class="theadThree">数量</view>
      </view>
      <view class="item" v-for="item in orderInfo.entries" :key="item.id">
        <view class="one">
          <view>{{ item.name }}</view>
          <view>{{ item.barCode }}</view>
        </view>
        <view class="two">¥{{ item.amount }}</view>
        <view class="three">{{ item.qty }}</view>
      </view>
      <view class="total">合计：{{ totalAmount }}</view>
    </view>
    <view class="confirmation">
      <view class="title">结算信息确认</view>
      <view class>
        <text class="left">本次付款</text>
        <input
          class="uni-input"
          type="number"
          v-model="rpAmount"
          placeholder-style="color:#CCCCCC"
          name="rpAmount"
          value
          :placeholder="totalAmount"
          @input="astrictMax"
          @blur="astrictMax"
        >
      </view>
      <view class>
        <text class="left">结算账户</text>
        <picker
          class="changes"
          @change="accountListPickerChange"
          :range="accountListArray"
          :value="accountListIndex"
          range-key="name"
        >
          <input
            type="text"
            :value="accountListPicker"
            disabled="true"
            class="uni-input"
            name
            placeholder="选择结算账户"
          >
        </picker>
        <view class="triangle"></view>
      </view>
      <view class>
        <text class="left">本次欠款</text>
        <text class>{{ arrears }}</text>
      </view>
      <view class="uni-btn-v">
        <button class @click="saveAdd">保存并新增</button>
      </view>
    </view>
  </view>
</template>

<script>
import { AccountList,InvoiceAdd } from "../../../interfaces.js";
export default {
  data() {
    return {
      userinfo: uni.getStorageSync("userinfo"),
      orderInfo: uni.getStorageSync("orderInfo").orderInfo,
      accountListArray: [],
      accountListIndex: 0,
      accountListPicker: "",

      rpAmount: ""
    };
  },
  computed: {
    // 计算合计
    totalAmount() {
      let totalSum = 0;
      this.orderInfo.entries.forEach(function(cur) {
        totalSum = totalSum + Number(cur.amount);
      });
      return totalSum.toFixed(1).toString();
    },
    // 计算欠款
    arrears() {
      return (this.totalAmount - this.rpAmount).toFixed(1);
    }
  },
  methods: {
    // 提交表单
    saveAdd() {
      // 结算账户
      console.log(this.orderInfo);
      this.orderInfo.accId = this.accountListArray[this.accountListIndex].id;
      if(this.accountListPicker.length == 0){
        uni.showToast({
          title: "请选择结算账户",
          icon: "none"
        });
        return;
      }
      // 付款金额
      this.orderInfo.rpAmount = this.rpAmount;
      if(this.orderInfo.rpAmount.length == 0){
        uni.showToast({
          title: "请填写付款金额",
          icon: "none"
        });
        return;
      }
      // 本次欠款
      this.orderInfo.arrears = Number(this.arrears);
      console.log(this.orderInfo);
      console.log(JSON.stringify(this.orderInfo));

      // 删除entries中的name和barCode
      for(var i in this.orderInfo.entries){
        delete this.orderInfo.entries[i].name;
        delete this.orderInfo.entries[i].barCode;
      }
      
      // 发送请求
      uni.request({
        method: "POST",
        url: InvoiceAdd,
        data: {
          database: this.userinfo.database,
          orderInfo:this.orderInfo
        },
        header: {
          "content-type": "application/x-www-form-urlencoded"
        },
        success:res => {
          console.log(res);
          debugger
          uni.showToast({
            title: res.data.msg,
            icon: "none"
          });
        }
      });
    },
    // 获取账户列表
    getAccountList() {
      uni.request({
        method: "GET",
        url: AccountList,
        data: {
          database: this.userinfo.database
        },
        success: res => {
          this.accountListArray = res.data.data;
        }
      });
    },
    // 限制本次付款最大值
    astrictMax() {
      if (Number(this.rpAmount) > Number(this.totalAmount)) {
        this.rpAmount = this.totalAmount;
      }
    },
    accountListPickerChange(e) {
      this.accountListIndex = e.target.value ;
      this.accountListPicker = this.accountListArray[
        this.accountListIndex
      ].name;
    }
    
  },
  onLoad() {
    this.getAccountList();
  }
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
.theadOne {
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
  text-align: center;
}
.theadTwo,
.two {
  flex: 1;
}
.two {
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
.confirmation {
  background-color: #fff;
  position: absolute;
  bottom: 0;
  width: 100%;
  font-size: 30upx;
}
.confirmation > view {
  border-bottom: 1px solid #dddddd;
  display: flex;
  align-items: center;
  height: 84upx;
}
.confirmation .title {
  justify-content: center;
  font-weight: 600;
}
.confirmation .uni-btn-v {
  height: 120upx;
  padding-right: 40upx;
  justify-content: flex-end;
}
.confirmation .left {
  width: 140px;
  text-align: center;
}
.uni-btn-v button {
  background-color: #136cae;
  color: #fff;
  height: 60upx;
  line-height: 60upx;
  width: 280upx;
  font-size: 30upx;
  margin: 0;
}
</style>
