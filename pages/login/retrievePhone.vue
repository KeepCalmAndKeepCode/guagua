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
        <input class="uni-input" placeholder-style = "color:#CCCCCC" name="tel" placeholder="请输入您预留的手机号码" />
      </view>
      <view class="uni-btn-v">
        <button class="next" formType="submit">下一步</button>
      </view>
    </form>

	</view>
</template>

<script>
  import { password } from "../../interfaces.js"
	export default {
		data() {
			return {
        userinfo: uni.getStorageSync("userinfo"),
				storename:""
			}
		},
		methods: {
			next(e){
        let tel = e.detail.value.tel;
        let storename = this.storename;
        uni.request({
          method:"POST",
          url:password,
           data:{
            // database: this.userinfo.database,
            storename,tel
          },
          header:{
            "content-type":"application/x-www-form-urlencoded"
          },
          success:res=>{
            console.log(res);
            if(res.data.code !== 200){
              uni.showToast({
                title:res.data.msg,
                icon:"none"
              });
            }else{
              uni.navigateTo({
                url: "retrieveGetpassword"
              });
            }
          }
        })
      }
    },
    onLoad(res){
      this.storename = res.storename;
    }
	}
</script>

<style scoped>
	page{
		background: #F2F2F2;
	}
	.procedure{
		font-size: 30upx;
		background-color: #FFFFFF;
		height: 65upx;
		line-height: 65upx;
		text-align: center;
	}
	.brackets{
		margin: 0 8upx;
	}
  .section .uni-input{
		background: #FFFFFF;
		font-size: 30upx;
		margin: 15upx 10upx;
		height: 81upx;
		line-height: 81upx;
		padding-left: 10upx;
	}
  .procedure2{
    color: #2C84CB;
  }
  .next{
    color: #fff;
		background: #2C84CB;
		margin: 10upx 10upx;
		font-size: 32upx;
  }
</style>
