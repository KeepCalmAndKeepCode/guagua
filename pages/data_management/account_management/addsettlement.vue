<template>
  <view id="app" class="container">
    <view class="custom">
      <label class="lab">账户编号</label>
      <input type="text" value v-model="number" placeholder="填写账户编号" class="inp">
    </view>
    <view class="custom">
      <label class="lab">账户名称</label>
      <input type="text" value v-model="name" placeholder="填写账户名称" class="inp">
    </view>
    <view class="uni-list-cell-db">
      <picker @change="bindPickerChange" :value="index" :range="array">
        <view class="accountType">
          <label class="lab">账户类别</label>
          <input
            type="text"
            :value="type"
            v-model="type"
            disabled="true"
            class="uni-input inp"
            placeholder="账户类别"
          >
        </view>
      </picker>
      <view class="triangle"></view>
    </view>
    <view class="custom">
      <label class="lab">账户余额</label>
      <input type="number"  value v-model="amount" placeholder="填写账户余额" class="inp">
    </view>
    <view class="section">
      <text class="title">余额日期</text>
      <picker class="datePicker two" mode="date" :value="date" @change="bindDateChange">
        <view class="uni-input">{{date}}</view>
      </picker>
      <text class="iconfont iconrili three"></text>
    </view>
    <button type="primary" class="btn btnfirst" @click="save">保存</button>
  </view>
</template>

<script>
import { AccountAdd } from "../../../interfaces.js";
export default {
  data() {
    const currentDate = this.getDate({
      format: true
    });
    return {
      userinfo: uni.getStorageSync("userinfo"),
      array: ["现金", "银行存款"],
      index: 0,
      type: "",
      name:"",
      amount:"",
      date: currentDate,
      number:""
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
  onNavigationBarButtonTap() {
    uni.navigateTo({
      url: "./addware"
    });
  },
  methods: {
    save() {
      let name = this.name;
      let number = this.number;
      let type = this.type == "现金"? 1 : 2;
      let amount = this.amount;
      let date = this.date;
      debugger
      if(!(name&&number&&date&&amount&&type)){
        uni.showToast({
          title: "请完善信息",
          icon: "none"
        });
        return
      }
      uni.request({
        method: "POST",
        url: AccountAdd,
        data: {
          database: this.userinfo.database,
          name,type,amount,date,number
        },
        header: {
          "content-type": "application/x-www-form-urlencoded"
        },
        success:res=>{
          console.log(res);
          debugger
          uni.showToast({
            title: res.data.msg,
            icon: "none"
          });
        }
      });
    },
    bindPickerChange: function(e) {
      this.index = e.target.value;
      this.type = this.array[this.index];
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
    }
  }
};
</script>

<style>
page {
  background: #fff;
  font-size: 30upx;
}
.container {
  padding: 20upx 45upx;
}
.custom {
  display: flex;
  justify-content: flex-start;
  line-height: 90upx;
  border-bottom: 1px solid #efefef;
}
.section {
  display: flex;
  align-items: center;
  height: 90upx;
  border-bottom: 1px solid #efefef;
}
.accountType {
  display: flex;
  justify-content: flex-start;
  line-height: 90upx;
}
.triangle {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 12upx 8upx 0;
  border-style: solid;
  border-color: #333 transparent transparent;
  /*灰 透明 透明 */
}
.lab,
.title {
  width: 200upx;
}
.inp {
  line-height: 90upx;
  height: 90upx;
}
.btn {
  width: 660upx;
  height: 75upx;
  line-height: 75upx;
  font-size: 30upx;
  margin: 30upx auto;
}
.uni-list-cell-db {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #efefef;
}
.btnfirst {
  background: #136cae;
  border: none;
  color: #fff;
}
.btnselecter {
  background: #8d8d8d;
  border: none;
  color: #fff;
}
.prompt {
  width: 600upx;
  background: #dfdfdf;
  margin: 240upx auto;
}
.ul {
  background: #fff;
}
.list {
  text-align: left;
  padding: 0 45upx;
  border-bottom: 1px solid #efefef;
}
.newcategory {
  background: #fff;
  width: 420upx;
  text-align: center;
  margin: 0 auto;
  font-size: 30upx;
}
.choice {
  display: flex;
  line-height: 75upx;
  margin-top: 45upx;
}
.bk01 {
  width: 50%;
  border-top: 1px solid #999;
}
.r {
  border-right: 1px solid #999;
}
.blue {
  color: #1c72b3;
}
</style>
