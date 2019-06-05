<template>
  <view class>
    <form @submit="formSubmit">
      <view class="item">
        <text class="tit">商品名称</text>
        <input type="text" :value="goodsName" name="goodsName" placeholder="选填或扫码添加商品">
        <text class="iconfont iconsaoyisaoerweimasaomasaomiao" @click="qrcode"></text>
      </view>
      <view class="item">
        <text class="tit">单位</text>
        <input type="text" value="" name="unit" placeholder>
      </view>
      <view class="item">
        <text class="tit">商品编码</text>
        <input type="text" name="commodityCode" value placeholder>
      </view>
      <view class="item">
        <text class="tit">登记证号</text>
        <input type="text" name="regNumber" value placeholder>
      </view>
      <view class="item">
        <text class="tit">仓库</text>
        <picker
          class="changes"
          @change="storagePickerChange"
          :range="storageArray"
          :value="storageIndex"
          mode="selector"
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
        <text class="tit">数量</text>
        <input type="text" value="" name="quantity" placeholder>
      </view>
      <view class="item">
        <text class="tit">购货单价</text>
        <input type="text" value="" name="unitPrice" placeholder>
      </view>
      <view class="item">
        <text class="tit">购货金额</text>
        <input type="text" name="amount" value placeholder>
      </view>
      <view class="total">未添加商品&emsp;合计：0.0</view>
      <view class="addAndClose">
        <button class="addProduct" @click="continueAdd">继续添加商品</button>
        <button class="closeAccount" formType="submit">去结算</button>
      </view>
    </form>
  </view>
</template>

<script>
export default {
  data() {
    return {
      storageArray: ["日泰仓储", "兴力仓储", "依轩仓储"],
      storageIndex: 0,
      storagePicker: "",

      chosen:"",

      goodsName:"",
    };
  },
  methods: {
    qrcode() {
			// 允许从相机和相册扫码
			uni.scanCode({
				success: res => {
					let url = res.result;
				}
			});
		},
    storagePickerChange(e) {
      this.storageIndex = e.target.value;
      this.storagePicker = this.storageArray[this.storageIndex];
    },
    formSubmit(e) {
      uni.navigateTo({
        url: "./new_sales"
      });
    },
    continueAdd(e) {
      this.goodsName = "";
      this.storageIndex = 0;
      this.storagePicker = "";
    }
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
</style>
