<!-- 客户 -->

<template>
	<view id="app" class="container">
		<view class="custom">
			<label class="lab">供应商编号</label>
			<input type="text" value="20180912151" placeholder="填写证件号码" class="inp" />
		</view>
		<view class="custom">
			<label class="lab">供应商名称</label>
			<input type="text" value="" placeholder="填写供应商名称" class="inp" />
		</view>
		<view class="custom lb">
			<label class="lab">供应商类别</label>
			<!-- <input type="text" value="" placeholder="选择供应商类别" id="click_down" class="inp xl" @click="togglePopup('bottom')" /> -->
			<input type="text" value="" placeholder="选择供应商类别" id="click_down" class="inp xl"  />
			<uni-popup :show="type === 'bottom'" position="bottom" mode="fixed" msg="">
				<!-- @hidePopup="togglePopup('')" -->
				<view class="prompt" v-show="tc">
					<view class="addcustom_1">
						<view class="text_c">添加客户类别</view>
						<input type="text" value="" placeholder="填写新的类别名称" class="newcategory" />
					</view>
					<view class="choice">
						<view v-for="(item, index) in confirmArray" :key="item.id" class="bk01" :class="{ r: index == 0, blue: index == 1 }" @click="confirm(index)">
							{{ item.name }}
						</view>
					</view>
				</view>
				<view class="ul">
					<view v-for="(item, index) in objectArray" :key="item.id" class="list" :class="{ blue: index == 0 }" @click="tanchaung(index)">{{ item.name }}</view>
				</view>
			</uni-popup>
			<view class="up_arrow"></view>
		</view>
		<!-- <button type="primary" class="" @click="bc">保存{{msgs}}</button>
		<button type="" id="primary2" v-show="button1">无需新增,跳过</button> -->
		<button type="primary" class="btn btnfirst" @click="bc">保存</button>
		<button class="btn btnselecter" v-show="button1">无需新增,跳过</button>
	</view>
</template>

<script>
import uniPopup from '../../components/uni-popup';

export default {
	components: {
		uniPopup
	},
	data() {
		return {
			type: '',
			tc: false,
			button1: false,
			msgs: '',
			confirmArray: [
				{
					id: 0,
					name: '取消'
				},
				{
					id: 1,
					name: '确认'
				}
			],
			objectArray: [
				{
					id: 0,
					name: '+添加类别'
				},
				{
					id: 1,
					name: '默认客户'
				},
				{
					id: 2,
					name: '默认客户2'
				}
			]
		};
	},
	methods: {
		tanchaung(index) {
			if (index !== 0) {
				return;
			}
			this.tc = true;
		},
		togglePopup(type) {
			this.type = type;
			if ((type = true)) {
				this.button1 = true;
				this.msgs = ',下一步';
			}
		},
		confirm(index) {
			if (index !== 1) {
				return;
			}
			this.tc = false;
			this.type = false;
			this.button1 = false;
			this.msgs = ' ';
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
	font-size: 36upx;
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
	font-size: 36upx;
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
