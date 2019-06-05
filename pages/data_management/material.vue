<template>
	<view class="mate">
		<view class="gl" @click="kh">客户管理</view>
		<view class="gl" @click="gys">供应商管理</view>
		<view class="gl" @click="ck">仓库管理</view>
		<view class="gl" @click="zh">账户管理</view>
	</view>
</template>

<script>
	export default{
		methods:{
			kh(){
				uni.navigateTo({
					url:'/pages/data_management/customer_management/data_management'
				})
			},
			gys(){
				uni.navigateTo({
					url:'/pages/data_management/supplier_management/supplier'
				})
			},
			ck(){
				uni.navigateTo({
					url:'/pages/data_management/warehouse_management/warehouse'
				})
			},
			zh(){
				uni.navigateTo({
					url:'/pages/data_management/account_management/account'
				})
			}
		}
	}
</script>

<style>
	.mate>*{
		font-size: 32upx;
	}
	.mate {
		padding: 0 20upx;
	}
	.gl{
		padding: 0 30upx;
		line-height: 85upx;
		border-bottom: 1upx solid #ddd;
	}
</style>
