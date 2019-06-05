<template>
  <view class>
    <form class="form" @submit="saveSkipSelectSupplier">
      <view class="section">
        <text class>供应商编号</text>
        <input
          class="uni-input"
          placeholder-style="font-size:30upx"
          name="number"
          placeholder="请输入供应商编号"
        >
      </view>
      <view class="section">
        <text class>供应商名称</text>
        <input
          class="uni-input"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="name"
          placeholder="填写供应商名称"
        >
      </view>
      <view class="section" @click="togglePopup('bottom-share')">
        <text class>供应商类别</text>
        <input
          class="uni-input"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="cCategory"
          placeholder="选择供应商类别"
          disabled="true"
          :value="optionSupplier.name"
        >
        <view class="triangle"></view>
      </view>
      <view class="uni-btn-v save">
        <button class="formSub" formType="submit">保存，下一步</button>
      </view>
      <view class="uni-btn-v jump" @click="skipSelectSupplier">
        <button class="formSub" formType>无需新增，跳过</button>
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
      <view class="classes add" @click="popupMid()">+ 添加类别</view>
      <view
        class="classes"
        v-for="item in supplier"
        :key="item.id"
        @click="optionSupplierType(item)"
      >{{item.name}}</view>
    </uni-popup>
  </view>
</template>

<script>
import uniPopup from "../../components/uni-popup";
import { ContactAdd, Category } from "../../../interfaces.js";
export default {
  components: {
    uniPopup
  },
  data() {
    return {
      userinfo: uni.getStorageSync("userinfo"),
      type: "",
      typeMid: false,
      supplier: [],
      optionSupplier: {}
    };
  },
  methods: {
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
    // 获取供应商类别
    getContact() {
      uni.request({
        method: "GET",
        url: Category,
        data: {
          database: this.userinfo.database,
          type: "supplytype"
        },
        success: res => {
          this.supplier = res.data;
        }
      });
    },
    // 选择供应商
    optionSupplierType(Supplier) {
      this.type = "";
      this.typeMid = false;
      this.optionSupplier = Supplier;
    },
    // 新增供应商
    saveSkipSelectSupplier(e) {
      let { name, number, cCategory } = e.detail.value;
      let cCategoryName = this.optionSupplier.id;
      if(!number){
        uni.showToast({
          title: "请输入供应商编号",
          icon: "none"
        });
        return
      }
      if(!name){
        uni.showToast({
          title: "请输入供应商名称",
          icon: "none"
        });
        return
      }
      if(!cCategory){
        uni.showToast({
          title: "请选择供应商类别",
          icon: "none"
        });
        return
      }
      uni.request({
        method: "POST",
        url: ContactAdd,
        data: {
          database: this.userinfo.database,
          number,
          name,
          cCategoryName,
          cCategory,
          type: 10
        },
        header: {
          "content-type": "application/x-www-form-urlencoded"
        },
        success: res => {
          console.log(res);
          uni.showToast({
            title: res.data.msg,
            icon: "none"
          });
          if (res.data.code == 200) {
            setTimeout(e => {
              uni.navigateTo({
                url: "selectSupplier"
              });
            }, 2000);
          }
        }
      });
    },
    skipSelectSupplier() {
      uni.navigateTo({
        url: "selectSupplier"
      });
    }
  },
  onLoad() {
    this.getContact();
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
.section > text {
  width: 260upx;
  text-align: center;
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
