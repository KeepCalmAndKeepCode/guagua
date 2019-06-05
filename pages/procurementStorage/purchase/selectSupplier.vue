<template>
  <view class>
    <form class="form" @submit="saveSkipSelectSupplier">
      <view class="section" @click="togglePopup('bottom-share')">
        <text class="title">供应商</text>
        <input
          class="uni-input two"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="newPassWord"
          placeholder="选择供应商"
          disabled="true"
          :value="selectSupplier.name"
        >
        <view class="triangle three"></view>
      </view>
      <view class="section">
        <text class="title">单据日期</text>
        <picker class="datePicker two" mode="date" :value="date" @change="bindDateChange">
          <view class="uni-input">{{date}}</view>
        </picker>
        <text class="iconfont iconrili three"></text>
      </view>
      <view class="section">
        <text class="title">单据编号</text>
        <input
          class="uni-input two"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="receiptNumber"
          placeholder="填写单据编号"
        >
      </view>
      <view class="uni-btn-v save">
        <button class="formSub" formType="submit">下一步</button>
      </view>
    </form>
    <view v-show="typeMid" class="middlePopup">
      <view class="containerMiddle">
        <view class="title">添加供应商类别</view>
        <input class="uni-input className" type="text" placeholder="填写新的类别名称">
        <view class="control">
          <view @click="closeMid()">取消</view>
          <view>保存</view>
        </view>
      </view>
    </view>
    <uni-popup
      :show="type === 'bottom-share'"
      position="bottom"
      mode="bottom"
      @hidePopup="togglePopup('')"
    >
      <view class="classes add" @click="popupMid()">+ 添加供应商</view>
      <view
        class="classes"
        v-for="(item , index) in suppliers"
        :key="item.id"
        @click="selectSupplierFun(index)"
      >{{ item.name }}</view>
    </uni-popup>
  </view>
</template>

<script>
import uniPopup from "../../components/uni-popup";
import { Contact } from "../../../interfaces.js";
export default {
  components: {
    uniPopup
  },
  data() {
    const currentDate = this.getDate({
      format: true
    });
    return {
      userinfo: uni.getStorageSync("userinfo"),
      type: "",
      typeMid: false,
      suppliers: [],
      selectSupplier: "",
      index: 0,
      date: currentDate
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
    // 获取供应商
    getSupplier() {
      uni.request({
        method: "GET",
        url: Contact,
        data: {
          database: this.userinfo.database,
          type: 10
        },
        success: res => {
          console.log(res);
          this.suppliers = res.data.data;
        }
      });
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
    bindDateChange(e) {
      this.date = e.target.value;
    },
    togglePopup(type) {
      this.type = type;
      this.typeMid = false;
    },
    popupMid() {
      this.typeMid = true;
    },
    closeMid() {
      this.typeMid = false;
    },
    selectSupplierFun(index) {
      this.selectSupplier = this.suppliers[index];
      this.type = "";
    },
    saveSkipSelectSupplier(e) {
      let billDate = this.date;
      let { receiptNumber } = e.detail.value;
      if(this.selectSupplier.length == 0){
        uni.showToast({
          title: "请选择供应商",
          icon: "none"
        });
        return;
      }
      if (receiptNumber.length == 0) {
        uni.showToast({
          title: "请填写单据编号",
          icon: "none"
        });
        return;
      }
      debugger;

      uni.setStorageSync("supplierInfo", {
        selectSupplier: this.selectSupplier,
        billDate,
        receiptNumber
      });
      uni.navigateTo({
        url: "./orderForm"
      });
    }
  },
  onLoad() {
    this.getSupplier();
  }
};
</script>

<style scope>
.section {
  display: flex;
  align-items: center;
  height: 90upx;
  border-bottom: #dddddd 1px solid;
  font-size: 15px;
}
.section > .title {
  width: 260upx;
  text-align: center;
}
.section > .two {
  flex: 1;
}
.section > .three {
  margin-right: 40upx;
}
.triangle {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 12upx 8upx 0;
  border-style: solid;
  border-color: #333 transparent transparent; /*灰 透明 透明 */
}
.save .formSub,
.jump .formSub {
  color: #fff;
  background-color: #136cae;
  font-size: 30upx;
  margin: 20upx;
}
.jump .formSub {
  background-color: #8d8d8d;
}
.classes {
  border-bottom: 1px solid #bbbbbb;
  height: 88upx;
  font-size: 30upx;
}
.add {
  color: #136cae;
}
.containerMiddle {
  width: 100%;
}
.containerMiddle > view {
  width: 100%;
}
.containerMiddle .title {
  font-weight: 600;
  text-align: center;
  height: 100upx;
  line-height: 100upx;
}
.className {
  background-color: #fff;
  width: 85%;
  margin: 0 auto 40upx;
  border: solid 1px #bbbbbb;
}
.middlePopup {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -100%);
  background-color: #dfdfdf;
  z-index: 999;
  width: 90%;
  font-size: 30upx;
}
.control {
  border-top: 1px solid #bbbbbb;
  display: flex;
  align-items: center;
  height: 80upx;
}
.control > view {
  width: 50%;
  text-align: center;
  line-height: 80upx;
}
.control :nth-child(2) {
  color: #136cae;
  border-left: 1px solid #bbbbbb;
}
</style>
