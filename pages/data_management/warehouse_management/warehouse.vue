<template>
	<view>
		<!-- <view class="uni-form-item uni-column">
			<text class="iconfont iconfangdajing"></text>
			<input class="uni-input inp1" focus placeholder="键入关键字查询" />
		</view> -->
		<view class="uni-block">
			<view class="uni-list">
				<view class="uni-list-cell">
					<view class="uni-list-cell-db">
						<picker @change="bindPickerChange" :value="index" :range="array">
							<input type="text" :value="picker" class="uni-input inp2" placeholder="编号" />
						</picker>
					</view>
				</view>
			</view>
			<view class="uni-list">
				<view class="uni-list-cell">
					<view class="uni-list-cell-db">
						<picker mode="date" :value="date" :start="startDate" :end="endDate" @change="bindDateChange">
							<input type="text" :value="timer" class="uni-input inp2" placeholder="添加时间" />
						</picker>
					</view>
				</view>
			</view>
		</view>
		<view v-for="(item,index) in customers" :key="item.id" class="uni-tab-bar">
			<view class="list">
				{{item.name}}
			</view>
			<view class="list">
				{{item.state}}
			</view>
			<!-- <view class="">
				{{item.timet}}
			</view> -->
		</view>
	</view>
</template>

<script>
	export default {

		data() {
			const currentDate = this.getDate({
				format: true
			})
			return {
				title: 'picker',
				array: ['中国', '美国', '巴西', '日本'],
				customers: [{
					'name': '001',
					'state': '默认仓库1',

				}, {
					'name': '002',
					'state': '默认仓库2',
				}],
				index: 0,
				date: currentDate,
				picker: '',
				timer: ''
			}
		},
		computed: {
			startDate() {
				return this.getDate('start');
			},
			endDate() {
				return this.getDate('end');
			}
		},
		onNavigationBarButtonTap() {
			uni.navigateTo({
				url: "./addware"
			});
		},
		methods: {
			
			bindPickerChange: function(e) {
				console.log('picker发送选择改变，携带值为', e.target.value)
				this.index = e.target.value,
					this.picker = this.array[this.index];
			},
			bindDateChange: function(e) {
				this.date = e.target.value
				this.timer = this.date
			},

			getDate(type) {
				const date = new Date();
				let year = date.getFullYear();
				let month = date.getMonth() + 1;
				let day = date.getDate();

				if (type === 'start') {
					year = year - 60;
				} else if (type === 'end') {
					year = year + 2;
				}
				month = month > 9 ? month : '0' + month;;
				day = day > 9 ? day : '0' + day;
				return `${year}-${month}-${day}`;
			}

		}
	}
</script>
<style>
	.uni-form-item {
		display: flex;
		justify-content: center;
		position: relative;
	}

	.inp1 {
		width: 600upx;
		border: 1px solid #C0C0C0;
		border-radius: 50upx;
		margin: 20upx 0;
		padding: 0 20upx;
		text-align: center;
		font-size: 36upx;
	}

	.inp2 {
		font-size: 36upx;
	}

	.uni-block {
		display: flex;
		justify-content: space-around;
		text-align: center;
	}

	.iconfont {
		position: absolute;
		display: flex;
		left: 25%;
		top: 30%;
		color: #C0C0C0;
		font-size: 36upx;
	}

	.uni-tab-bar {
		display: flex;
		justify-content: space-around;
		font-size: 32upx;
		line-height: 75upx;
		border-bottom: 1px solid #f7f7f7;
	}
	.list{
		width: 50%;
		text-align: center;
	}
</style>
