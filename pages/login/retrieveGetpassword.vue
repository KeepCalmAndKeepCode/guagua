<template>
  <view class="container">
    <view class="procedure">
      <text class="procedure1">1.输入店铺名称</text>
      <text class="brackets">></text>
      <text class="procedure2">2.请输入手机号</text>
      <text class="brackets">></text>
      <text class="procedure3">3.获取新密码</text>
    </view>
    <view class="section">新密码将以短信形式发送至您的手机，请注意查收。</view>
    <form class="form" @submit="next">
      <view class="uni-btn-v">
        <button class="next" formType="submit">返回登录页</button>
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
    next() {
      uni.navigateTo({
        url: "login"
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
.section {
  text-align: center;
  font-size: 30upx;

  height: 100upx;
  line-height: 100upx;
}
.procedure3 {
  color: #2c84cb;
}
.next {
  color: #fff;
  background: #2c84cb;
  margin: 10upx 10upx;
  font-size: 32upx;
}
</style>
