<template>
  <view class="container">
		<image src="../../static/logo.png" mode="" class="logo"></image>
		<view class="ggnz">
			呱呱农资
		</view>
    <form class="form" @submit="formSubmit">
    	<view class="section">
    		<input class="uni-input" placeholder-style = "color:#CCCCCC" name="storename" placeholder="输入店铺名称" />
    	</view>
			<view class="section">
				<input class="uni-input"  placeholder-style="color:#CCCCCC" name="username" placeholder="输入域账号" />
			</view>
			<view class="section">
				<input class="uni-input"  placeholder-style="color:#CCCCCC" password name="userpwd" placeholder="输入域密码" />
			</view>
			<view class="uni-btn-v">
				<button class="formSub" formType="submit">登录</button>
			</view>
			<view class="retrieve" @click="retrieve">
				找回密码
			</view>
    </form>
  </view>
</template>

<script>
import { login } from "../../interfaces.js";
	export default {
		data(){
			return{
				
			}
		},
		onNavigationBarButtonTap(e) {
				uni.navigateTo({
          url: 'set'
				});
			},
		methods:{
			formSubmit(e){
        console.log(e);
        let { storename,username,userpwd } = e.detail.value;
        if(!storename){
          uni.showToast({
            title:"请输入店铺名",
            icon:"none"
          });
          return
        }
        if(!username){
          uni.showToast({
            title:"请输入账号",
            icon:"none"
          });
          return
        }
        if(!userpwd){
          uni.showToast({
            title:"请输入密码",
            icon:"none"
          });
          return
        }
        uni.request({
          method:"POST",
          url:login,
          data:{
            storename,username,userpwd
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
              uni.showToast({
                title:"登录成功",
                icon:"none"
              });
            }
            let database = res.data.database;
            let uid = res.data.uid;
            uni.setStorageSync("userinfo",{
              storename,username,userpwd,database,uid
            })
            setTimeout(()=>{
              uni.navigateTo({
                url: "/pages/home/home"
              });
            },2000)
            
          }
        })
				
			},
			retrieve(){
				uni.navigateTo({
					url: "retrieveShop"
				});
			}
		}
	}
</script>

<style scope>
	.container{
		padding-top: 120upx;
	}
	.title{
		background: #f8f8f8;
	}
	.logo{
    width: 200upx;
    height: 200upx;
    margin: 0 auto 10upx;
    display: block;
  }
	.ggnz{
		font-family: SourceHanSansSC;
    font-weight: 400;
    font-size: 50upx;
    color: #1C72B3;
    font-style: normal;
    letter-spacing: 0px;
    text-decoration: none;
		text-align: center;
	}
	.form .section .uni-input{
		background: #F2F2F2;
		font-size: 30upx;
		margin: 15upx 10upx;
		height: 81upx;
		line-height: 81upx;
		padding-left: 10upx;
	}
	.formPlaceholder{
		color: #FFFFFF;
	}
	.uni-btn-v .formSub{
		color: #fff;
		background: #2C84CB;
		margin: 10upx 10upx;
		font-size: 32upx;
	}
	
	.retrieve{
		color: #2C84CB;
		font-size: 32upx;
		text-align: center;
	}
</style>
