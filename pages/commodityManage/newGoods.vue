<template>
  <view class="content">
    <form class="form" @submit="onNavigationBarButtonTap">
      <view class="item">
        <view class="items">
          <text class="required">*</text>
          <text class>登记证号</text>
        </view>

        <input
          class
          type="text"
          v-model="number"
          name="number"
          placeholder="填写登记证号"
          placeholder-style="color:#BBBBBB"
        >
      </view>
      <view class="item">
        <view class="items">
          <text class="required">*</text>
          <text class>商品名称</text>
        </view>
        <input
          class
          type="text"
          v-model="name"
          placeholder="填写商品名称"
          placeholder-style="color:#BBBBBB"
        >
      </view>
      <view class="item">
        <view class="items">
          <text class="required">*</text>
          <text class>商品编码</text>
        </view>
        <input
          class
          type="text"
          v-model="barCode"
          name="barCode"
          placeholder="填写商品编码"
          placeholder-style="color:#BBBBBB"
        >
      </view>
      <view class="item">
        <view class="items">
          <text class="required">*</text>
          <text class>商品类别</text>
        </view>
        <picker
          class="changes"
          @change="GoodsTypePickerChange"
          :value="GoodsTypeIndex"
          :range="GoodsTypeArray"
          range-key="name"
        >
          <input
            type="text"
            :value="GoodsTypePicker"
            disabled="true"
            class="uni-input"
            placeholder="商品类别"
            name="categoryName"
          >
        </picker>
      </view>
      <view class="item">
        <view class="items">
          <text class="required">*</text>
          <text class>首选仓库</text>
        </view>
        <picker
          class="changes"
          @change="storagePickerChange"
          :value="storageIndex"
          :range="storageArray"
          range-key="name"
        >
          <input
            type="text"
            :value="storagePicker"
            disabled="true"
            class="uni-input"
            placeholder="选择仓库"
          >
        </picker>
      </view>
      <view class="item">
        <view class="items">
          <text class="required">*</text>
          <text class>零售单位</text>
        </view>
        <picker
          class="changes"
          @change="unitPickerChange"
          :value="unitIndex"
          :range="unitArray"
          range-key="name"
        >
          <input
            type="text"
            :value="unitPicker"
            disabled="true"
            class="uni-input"
            placeholder="选择零售单位"
          >
        </picker>
      </view>
      <view class="item">
        <text class="required">*</text>
        <text class="items">整箱单位</text>
        <picker
          class="changes"
          @change="caseunitPickerChange"
          :value="caseunitIndex"
          :range="caseunitArray"
          range-key="name"
        >
          <input
            type="text"
            :value="caseunitPicker"
            disabled="true"
            class="uni-input"
            placeholder="选择整箱单位"
          >
        </picker>
      </view>
      <view class="item">
        <text class="required">*</text>
        <text class="items">整箱数量</text>
        <input class type="text" v-model="bnum" placeholder="输入整箱数量" placeholder-style="color:#BBBBBB">
      </view>
      <view class="item">
        <text class="required">*</text>
        <text class="items">生产公司</text>
        <input class type="text" v-model="company" placeholder="输入生产公司" placeholder-style="color:#BBBBBB">
      </view>
      <view class="item">
        <text class="items">规格型号</text>
        <input class type="text" v-model="spec" placeholder="填写规格型号" placeholder-style="color:#BBBBBB">
      </view>
      <view class="item">
        <text class="items">零售价</text>
        <input class type="text" v-model="salePrice" placeholder="输入零售价" placeholder-style="color:#BBBBBB">
      </view>
    </form>
  </view>
</template>

<script>
import { Category, StorageList, Unit, addGoods } from "../../interfaces.js";
export default {
  data() {
    return {
      userinfo: uni.getStorageSync("userinfo"),
      GoodsTypeArray: [],
      GoodsTypeIndex: 0,
      GoodsTypePicker: "",

      storageArray: [],
      storageIndex: 0,
      storagePicker: "",

      unitArray: [],
      unitIndex: 0,
      unitPicker: "",

      caseunitArray: [],
      caseunitIndex: 0,
      caseunitPicker: "",

      number:"",
      name:"",
      barCode:"",
      categoryName:"",
      baseUnitId:"",
      unitName:"",
      spec:"",
      locationId:"",
      locationName:"",
      salePrice:"",
      bnum:"",
      bunitName:"",
      company:"",
      bunit:""

    };
  },
  methods: {
    GoodsTypePickerChange(e) {
      this.GoodsTypeIndex = e.target.value;
      this.GoodsTypePicker = this.GoodsTypeArray[this.GoodsTypeIndex].name;
    },
    storagePickerChange(e) {
      this.storageIndex = e.target.value;
      this.storagePicker = this.storageArray[this.storageIndex].name;
    },
    unitPickerChange(e) {
      this.unitIndex = e.target.value;
      this.unitPicker = this.unitArray[this.unitIndex].name;
    },
    caseunitPickerChange(e) {
      this.caseunitIndex = e.target.value;
      this.caseunitPicker = this.caseunitArray[this.caseunitIndex].name;
    },
    // 获取分类
    getCategory() {
      uni.request({
        method: "GET",
        url: Category,
        data: {
          database: this.userinfo.database,
          type: "category"
        },
        success: res => {
          console.log(res.data);
          this.GoodsTypeArray.push(...res.data);
        }
      });
    },
    // 获取所有仓库
    getStorage() {
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
    // 获取所有零售单位
    getUnit() {
      uni.request({
        method: "GET",
        url: Unit,
        data: {
          database: this.userinfo.database
        },
        success: res => {
          this.unitArray = res.data.data;
          this.caseunitArray = res.data.data;
        }
      });
    },
    // 提交
    formSubmit(e) {
    }
  },
  // 提交
  onNavigationBarButtonTap(){
    let number = this.number;// 登记证号
    let name = this.name;   // 商品名称
    let barCode = this.barCode;// 商品编码
    let categoryId = this.GoodsTypeArray[this.GoodsTypeIndex].id; // 商品分类ID
    let categoryName = this.GoodsTypePicker;// 商品分类名称
    let baseUnitId = this.unitArray[this.unitIndex].id; // 计量（零售）单位ID
    let unitName = this.unitPicker; // 计量（零售）单位名称
    let locationId = this.storageArray[this.storageIndex].id; // 仓库ID
    let locationName = this.storagePicker; // 仓库名称
    let bunit = this.caseunitArray[this.caseunitIndex].id; // 整箱单位id
    let bunitName = this.caseunitPicker;  // 整箱单位名称
    let bnum = this.bnum; // 整箱数量
    let company = this.company; // 生产公司
    if(!(number&&name&&barCode&&categoryId&&categoryName&&unitName&&locationName&&bunitName&&bnum&&company)){
      uni.showToast({
        title:"请完善信息",
        icon:"none"
      });
      return
    }
    let spec = this.spec; // 规格型号
    let salePrice = this.salePrice;  // 零售价
    uni.request({
      method: "POST",
      url: addGoods,
       header:{
        "content-type":"application/x-www-form-urlencoded"
      },
      data: {
        database: this.userinfo.database,
        number,name,barCode,categoryId,categoryName,baseUnitId,unitName,spec,locationId,locationName,salePrice,bnum,bunitName,company,bunit
      },
      success: res => {
        console.log(res);
        uni.showToast({
          title:res.data.msg,
          icon:"none"
        });
        setTimeout(()=>{
          uni.navigateTo({
            url: "/pages/commodityManage/commodityManage"
          });
        },2000)
      }
    });
  },
  onLoad(option) {
    // if (option.commodityInfo != "undefined") {
    //   uni.setNavigationBarTitle({
    //     title: "编辑商品"
    //   });
    // }
    this.getCategory();
    this.getStorage();
    this.getUnit();
  }
};
</script>

<style scoped>
.content .form > view {
  display: flex;
  align-items: center;
  font-size: 20upx;
}
.form {
  font-size: 30upx;
}
.form .item {
  padding-left: 40upx;
  display: flex;
  align-items: center;
  height: 80upx;
  border-bottom: 1px solid #dddddd;
}
.item .changes {
  border: none;
  width: 400upx;
  height: 100%;
  display: flex;
  align-items: center;
}
.required {
  color: #e6242b;
  margin-right: 10upx;
}
.items {
  width: 200upx;
}
</style>
