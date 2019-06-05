<template>
  <view class="container">
  	<form class="form" @submit="formSubmit">
			<view class="section">
    		<input class="uni-input" placeholder-style = "color:#CCCCCC;font-size:30upx" name="newPassWord" placeholder="输入新密码" />
    	</view>
			<view class="section">
				<input class="uni-input"  placeholder-style="color:#CCCCCC;font-size:30upx" name="newPassWord" placeholder="确认新密码" />
			</view>
      <view class="annotation">长度8~20位，同时包含数字、字母（区分大小写），可使用特殊符号</view>
      <view class="uni-btn-v">
				<button class="formSub" formType="submit">确认提交</button>
			</view>
    </form>
  </view>
</template>

<script>
export default {

}
</script>

<style scope>
page {
  background-color: #f2f2f2;
}
.container {
  padding: 20upx 25upx;
}
.container .section {
  background-color: #fff;
  height: 90upx;
  line-height: 90upx;
  display: flex;
  align-items: center;
  margin-bottom: 15upx;
}
.form .section .uni-input {
  height: 90upx;
  line-height: 90upx;
  flex: 1;
  padding-left: 20upx;
}
.annotation{
  font-size: 23upx;
  color: #A9A9A9;
}
.uni-btn-v .formSub{
  color: #fff;
  background: #2C84CB;
  margin: 10upx 0;
  font-size: 32upx;
}
</style>
