<template>
  <view class>
    <form @submit="formSubmit">
      <view class="item">
        <text class="tit">商品名称</text>
        <picker
          class="changes"
          @change="goodsPickerChange"
          :range="goodsArray"
          :value="goodsIndex"
          range-key="name"
        >
          <input
            type="text"
            :value="goodsPicker"
            disabled="true"
            class="uni-input"
            name="storage"
            placeholder="选择商品"
          >
        </picker>
        <text class="iconfont iconsaoyisaoerweimasaomasaomiao" @click="scanCode"></text>
      </view>
      <view class="item">
        <text class="tit">商品单价</text>
        <input type="number"  name="price" v-model="price">
      </view>
      <view class="item">
        <text class="tit">单商品数量</text>
        <input type="number"  name="qty" v-model="qty">
      </view>
      <view class="item">
        <text class="tit">整箱数量</text>
        <input type="number"  name="bnum" v-model="bnum">
      </view>

      <view class="item">
        <text class="tit">生产公司</text>
        <input type="text"  name="bnum" v-model="company">
      </view>
      <!-- <view class="item" v-show="accountShow">
        <text class="tit">结算账户</text>
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
            name="storage"
            placeholder="选择结算账户"
          >
        </picker>
        <view class="triangle"></view>
      </view> -->
      <view class="item">
        <text class="tit">仓库</text>
        <picker
          class="changes"
          @change="storagePickerChange"
          :range="storageArray"
          :value="storageIndex"
          range-key="name"
        >
          <input
            type="text"
            :value="storagePicker"
            disabled="true"
            class="uni-input"
            name="storage"
            placeholder="选择仓库"
          >
        </picker>
        <view class="triangle"></view>
      </view>

      <view class="item">
        <text class="tit">生产日期</text>
        <picker class="datePicker two" mode="date" :value="date" @change="bindDateChange">
          <view class="uni-input">{{date}}</view>
        </picker>
        <text class="iconfont iconrili three"></text>
      </view>
      <view class="total">购货金额：{{ amount }}</view>
      <view class="addAndClose">
        <button class="addProduct" @click="continueAdd">继续添加商品</button>
        <button class="closeAccount" formType="submit">去结算</button>
      </view>
    </form>
  </view>
</template>

<script>
import { Goods, StorageList } from "../../../interfaces.js";
export default {
  data() {
    const currentDate = this.getDate({
      format: true
    });
    return {
      userinfo: uni.getStorageSync("userinfo"),
      supplierInfo: uni.getStorageSync("supplierInfo"),
      date: currentDate,
      // accountShow: true,
      control:false,

      goodsArray: [],
      goodsIndex: 0,
      goodsPicker: "",

      storageArray: [],
      storageIndex: 0,
      storagePicker: "",

      // accountListArray: [],
      // accountListIndex: 0,
      // accountListPicker: "",

      postData: {
        entries: []
      },

      qty: "",
      bnum: "",
      price: "",
      company: ""
    };
  },
  computed: {
    amount() {
      return this.price * this.qty;
    },
    startDate() {
      return this.getDate("start");
    },
    endDate() {
      return this.getDate("end");
    }
  },
  methods: {
    // 校验并添加至postData公共方法
    checkAdd(){
      /**
       * 第一步判断字段是否填写完整
       */
      // 校验商品名称
      if (this.goodsPicker.length == 0) {
        uni.showToast({
          title: "请选择商品",
          icon: "none"
        });
        return;
      }
      if(this.price.length == 0){
        uni.showToast({
          title: "请填写商品单价",
          icon: "none"
        });
        return;
      }
      // 校验仓库
      if (this.storagePicker.length == 0) {
        uni.showToast({
          title: "请选择仓库",
          icon: "none"
        });
        return;
      }
      // 如果已选择结算账户则隐藏此字段
      // if (this.accountListPicker.length == 0) {
      //   uni.showToast({
      //     title: "请选择结算账户",
      //     icon: "none"
      //   });
      //   return;
      // } else {
      //   this.accountShow = false;
      // }

      // 校验生产日期
      if (this.date.length == 0) {
        uni.showToast({
          title: "请选择生产日期",
          icon: "none"
        });
        return;
      }
      // 校验生产公司
      if (this.company.length == 0) {
        uni.showToast({
          title: "请填写生产公司",
          icon: "none"
        });
        return;
      }
      // 校验整箱数量
      if (this.bnum.length == 0) {
        uni.showToast({
          title: "请填写整箱数量",
          icon: "none"
        });
        return;
      }
      // 校验单商品数量
      if(this.qty.length == 0){
        uni.showToast({
          title: "请填写单商品数量",
          icon: "none"
        });
        return;
      }

      /**
       * 控制代码是否向下走
       */
      this.control = true;

      /**
       * 第二步将商品信息添加至postData.entries
       */
      let invIdent = this.goodsArray[this.goodsIndex].id; // 商品ID
      let barCode = this.goodsArray[this.goodsIndex].barCode;// 暂时把商品编号和名称传过去
      let name = this.goodsArray[this.goodsIndex].name;// 暂时把商品编号和名称传过去
      let qty = this.qty; // 单商品数量
      let price = this.price; // 商品单价
      let amount = this.amount; // 购货金额
      let locationId = this.storageArray[this.storageIndex].id; // 仓库ID
      let bnum = this.bnum; // 整箱数量
      let company = this.company; // 生产公司
      let mdate = this.date; // 生产日期
      // 此页面并不需要填写结算账户所以此页面可删除所有结算账户相关字段
      // this.postData.accId = this.accountListArray[this.accountListIndex].id; // 结算账户ID
      this.postData.entries.push({
        invId: invIdent,
        barCode,
        name,
        qty,
        price,
        amount,
        locationId,
        bnum,
        company,
        mdate,
        discountRate: 0,
        deduction: 0
      });
    },
    // 继续添加商品
    continueAdd(e) {
      this.checkAdd();

      // 添加流程控制，否则会清空表单
      if(this.control == false){
        return
      }
      /**
       * 最后清空所填信息
       */
      this.goodsPicker = ""; // 清空商品名称
      this.storagePicker = ""; // 清空仓库名称
      this.price = ""; // 清空商品单价
      this.qty = ""; // 单商品数量
      this.bnum = ""; // 清空整箱数量
      this.company = ""; // 清空生产公司

      this.control = false; // 控制代码流程
    },
    // 去结算
    formSubmit(e) {
      console.info(this.postData);
      this.checkAdd();

      this.postData.transType = "150501";                            // 类型（150501购货 150502购货退货  150601销货 150602销货退货单）
      this.postData.invId = this.postData.entries[0].invId;          // 商品ID
      this.postData.buId = this.supplierInfo.selectSupplier.id;      // 供应商id
      this.postData.billDate = this.supplierInfo.billDate;           // 订单日期
      let total = 0; 
      this.postData.entries.forEach(function(cur){
        total = total + cur.amount;
      })
      this.postData.totalAmount = total;                             // 总价格
      this.postData.amount = total;
      let totalQty = 0;
      this.postData.entries.forEach(function(cur){
        totalQty = totalQty + Number(cur.qty);
        debugger
      })
      console.log(totalQty);
      debugger
      this.postData.totalQty = totalQty;                            // 总数量
      this.postData.arrears = "待定";                               // 本次欠款（需传到下个页面使用）
      this.postData.rpAmount = "待定";                              // 付款金额

      console.log(this.postData);
      
      uni.setStorageSync('orderInfo',{
        orderInfo:this.postData
      })
      uni.navigateTo({
        url: "./closeAccount"
      });
    },

    // 扫码
    scanCode() {
      alert("扫码");
    },
    // 获取商品列表
    getGoods() {
      uni.request({
        method: "GET",
        url: Goods,
        data: {
          database: this.userinfo.database
        },
        success: res => {
          this.goodsArray = res.data.data;
        }
      });
    },
    // 获取仓库
    getStorageList() {
      uni.request({
        method: "GET",
        url: StorageList,
        data: {
          database: this.userinfo.database
        },
        success: res => {
          this.storageArray = res.data.data;
        }
      });
    },
    // // 获取账户列表
    // getAccountList() {
    //   uni.request({
    //     method: "GET",
    //     url: AccountList,
    //     data: {
    //       database: this.userinfo.database
    //     },
    //     success: res => {
    //       this.accountListArray = res.data.data;
    //     }
    //   });
    // },
    goodsPickerChange(e) {
      this.goodsIndex = e.target.value || 0;
      this.goodsPicker = this.goodsArray[this.goodsIndex].name;
    },
    storagePickerChange(e) {
      this.storageIndex = e.target.value || 0;
      this.storagePicker = this.storageArray[this.storageIndex].name;
    },
    // accountListPickerChange(e) {
    //   this.accountListIndex = e.target.value || 0;
    //   this.accountListPicker = this.accountListArray[this.accountListIndex].name;
    // },
    bindDateChange(e) {
      this.date = e.target.value;
      this.timer = this.date;
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
  },
  onLoad() {
    this.getGoods();
    this.getStorageList();
    // this.getAccountList();
  }
};
</script>

<style scoped>
.triangle {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 12upx 8upx 0;
  border-style: solid;
  border-color: #333 transparent transparent;
  /*灰 透明 透明 */
}

.item {
  display: flex;
  align-items: center;
  border-bottom: 1upx solid #dddddd;
  height: 80upx;
  padding-left: 60upx;
  font-size: 30upx;
}

.tit {
  width: 200upx;
}

.total {
  color: #136cae;
  display: flex;
  justify-content: flex-end;
  padding: 30upx 30upx 0 0;
  font-size: 36upx;
  font-weight: 600;
}

.addAndClose {
  position: fixed;
  bottom: 20upx;
  color: #ffffff;
  width: 100%;
}

.addAndClose > button {
  width: 95%;
  margin: 0 auto;
  text-align: center;
  font-size: 30upx;
  padding: 10upx 0;
  border-radius: 10upx;
  color: #ffffff;
}

.addProduct {
  background-color: #136cae;
}

.addAndClose .closeAccount {
  background-color: #eb7605;
  margin-top: 16upx;
}
.two {
  width: 356upx;
}
</style>
