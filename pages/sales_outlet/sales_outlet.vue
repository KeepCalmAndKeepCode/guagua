<template>
	<view class="container">
		<view v-for="(list,index) in lists" :key="index" class="list" @click="skip(list.skip)">
			<text class="icons iconfont" :class="list.tubiao"></text>
			<view class="bill">{{list.name}}</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				lists: [{
					skip: "./new_sales_list/purchase",
					tubiao: "iconxinzeng",
					name: '新增售货单'
				}, {
					skip: "/pages/sales_outlet/querySales",
					tubiao: "iconchaxun c2",
					name: '查询销货单',
				}, {
					skip: "./return/purchase",
					tubiao: "iconxinzeng",
					name: '新增销货退货单'
				}, {
					skip: "",
					tubiao: "iconchaxun c2",
					name: '查询销货退货单'
				}],
			}
		},
		methods: {
			skip(url) {
				uni.navigateTo({
					url
				});
			}
		}
	}
</script>

<style>
	page{
		background: #EFEFEF;
	}
	.container{
		background: #fff;
	}
	.list{
		line-height: 200upx;
		display: flex;
		justify-content: center;
		border-bottom: 1px solid #EFEFEF;
	}
	.icons{
		font-size: 120upx;
		padding-right: 60upx;
		color: #136cae;
	}
	.iconxinzeng{
		font-size: 105upx;
	}
	.bill{
		width:275upx;
		text-align: left;
		color: #136CAE;
	}
</style>
