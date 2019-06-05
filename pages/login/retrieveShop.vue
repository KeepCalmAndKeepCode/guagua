<template>
  <view class="container">
    <view class="procedure">
      <text class="procedure1">1.输入店铺名称</text>
      <text class="brackets">></text>
      <text class="procedure2">2.请输入手机号</text>
      <text class="brackets">></text>
      <text class="procedure3">3.获取新密码</text>
    </view>
    <form class="form" @submit="next">
      <view class="section">
        <input
          class="uni-input"
          placeholder-style="color:#CCCCCC"
          name="storename"
          placeholder="请输入您的店铺名称（请与营业执照保持一致）"
        >
      </view>
      <view class="uni-btn-v">
        <button class="next" formType="submit">下一步</button>
      </view>
    </form>
  </view>
</template>

<script>
export default {
  data() {
    return {};
  },
  methods: {
    next(e) {
      uni.navigateTo({
        url: "retrievePhone?storename=" + e.detail.value.storename
      });
    }
  }
};
</script>

<style scoped>
page {
  background: #f2f2f2;
}
.procedure {
  font-size: 30upx;
  background-color: #ffffff;
  height: 65upx;
  line-height: 65upx;
  text-align: center;
}
.brackets {
  margin: 0 8upx;
}
.section .uni-input {
  background: #ffffff;
  font-size: 30upx;
  margin: 15upx 10upx;
  height: 81upx;
  line-height: 81upx;
  padding-left: 10upx;
}
.procedure1 {
  color: #2c84cb;
}
.next {
  color: #fff;
  background: #2c84cb;
  margin: 10upx 10upx;
  font-size: 32upx;
}
</style>
