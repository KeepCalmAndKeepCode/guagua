<!-- 客户 -->
<template>
	<view id="app" class="container">
		<view class="custom">
			<label class="lab">仓库编号</label>
			<input type="text" value="" v-model="locationNo" name="locationNo" placeholder="填写仓库编号" class="inp" />
		</view>
		<view class="custom">
			<label class="lab">仓库名称</label>
			<input type="text" value="" v-model="name" name="name" placeholder="填写仓库名称" class="inp" />
		</view>
		<button type="primary" class="btn btnfirst" @click="storageAdd()">保存</button>
	</view>
</template>

<script>
import uniPopup from '../../components/uni-popup';
import { StorageAdd } from "../../../interfaces.js";
export default {
	components: {
		uniPopup
	},
	data() {
		return {
      userinfo: uni.getStorageSync("userinfo"),
			type: '',
      msgs: '',
      locationNo:"",
      name:"",
			confirmArray: [
				{
					id: 0,
					name: '取消'
				},
				{
					id: 1,
					name: '确认'
				}
			]
		};
	},
	methods: {
    // 添加仓库
    storageAdd(){
      let locationNo = this.locationNo;
      let name = this.name;
      if(!(locationNo&&name)){
        uni.showToast({
          title:"请完善填写信息",
          icon: "none"
        });
      }
      uni.request({
        method:"POST",
        url:StorageAdd,
        data:{
          database: this.userinfo.database,
          locationNo,
          name
        },
        header: {
          "content-type": "application/x-www-form-urlencoded"
        },
        success:res=>{
          uni.showToast({
            title: res.data.msg,
            icon: "none"
          });
        }
      })
    },
		bc() {
			uni.navigateTo({
				url: './data_management'
			});
		}
	}
};
</script>

<style>
page {
	background: #fff;
	font-size: 30upx;
}
.container {
	padding: 20upx 45upx;
}
.custom {
	display: flex;
	justify-content: flex-start;
	line-height: 90upx;
	border-bottom: 1px solid #efefef;
}
.lab {
	width: 275upx;
}
.inp {
	line-height: 90upx;
	height: 90upx;
}
.btn {
	width: 660upx;
	height: 75upx;
	line-height: 75upx;
	font-size: 30upx;
	margin: 30upx auto;
}
.btnfirst {
	background: #136cae;
	border: none;
	color: #fff;
}
.btnselecter {
	background: #8d8d8d;
	border: none;
	color: #fff;
}
.prompt {
	width: 600upx;
	background: #dfdfdf;
	margin: 240upx auto;
}
.ul {
	background: #fff;
}
.list {
	text-align: left;
	padding: 0 45upx;
	border-bottom: 1px solid #efefef;
}
.newcategory {
	background: #fff;
	width: 420upx;
	text-align: center;
	margin: 0 auto;
	font-size: 30upx;
}
.choice {
	display: flex;
	line-height: 75upx;
	margin-top: 45upx;
}
.bk01 {
	width: 50%;
	border-top: 1px solid #999;
}
.r {
	border-right: 1px solid #999;
}
.blue {
	color: #1c72b3;
}
</style>
