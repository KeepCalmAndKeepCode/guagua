<template>
  <view class="content">
  	<view class="" @click="skipChangePass">
			<text class="one">修改密码</text>
  	</view>
		<view class="">
			<text class="one">清理缓存</text>
			<text class="two">559K</text>
		</view>
		<view class="">
			<text class="one">检查新版本</text>
			<text class="two">1.3.1</text>
		</view>
		<view class="last">
			<text class="one">退出登录</text>
		</view>
  </view>
</template>

<script>
export default {
	methods:{
    skipChangePass(){
      uni.navigateTo({
        url: "/pages/home/changePass"
      });
    }
  }
}
</script>

<style scoped>
.content {
  border-top: 1px #ddd solid;
  border-bottom: 1px #ddd solid;
  padding: 15upx;
}
.content > view {
  border-bottom: 1px #ddd solid;
  height: 90upx;
  display: flex;
  align-items: center;
  padding: 0 20upx;
}
.content > view .one{
  flex: 1;
}
.content > view .two{
  color: #707070;
}
.content  .last {
  border: none;
}

</style>
