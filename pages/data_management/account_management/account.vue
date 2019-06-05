<template>
  <view>
    <view class="uni-block">
      <view class="uni-list">
        <view class="uni-list-cell-db">
          <text class>编号</text>
          <!-- <view class="triangle"></view> -->
        </view>
      </view>
      <view class="uni-list">
        <view class="uni-list-cell-db">
          <text class>添加时间</text>
          <!-- <view class="triangle"></view> -->
        </view>
      </view>
      <view class="uni-list">
        <view class="uni-list-cell-db">
          <text class>账户类别</text>
          <!-- <view class="triangle"></view> -->
        </view>
      </view>
      <view class="uni-list">
        <view class="uni-list-cell-db">
          <text class>账户余额</text>
          <!-- <view class="triangle"></view> -->
        </view>
      </view>
    </view>
    <view v-for="item in accountListArray" :key="item.id" class="uni-tab-bar">
      <view class="list">{{ item.name }}</view>
      <view class="list">{{ item.date }}</view>
      <view class="list">{{ item.state }}</view>
      <view class="list">{{ item.amount }}</view>
    </view>
  </view>
</template>

<script>
import { AccountList  } from "../../../interfaces.js";
export default {
  data() {
    const currentDate = this.getDate({
      format: true
    });
    return {
      userinfo: uni.getStorageSync("userinfo"),
      accountListArray:[]
    };
  },
  computed: {
    startDate() {
      return this.getDate("start");
    },
    endDate() {
      return this.getDate("end");
    }
  },
  onNavigationBarButtonTap() {
    uni.navigateTo({
      url: "./addsettlement"
    });
  },
  methods: {
    // 获取账户列表
    getAccountList(){
      uni.request({
        method: "GET",
        url:AccountList,
        data:{
          database: this.userinfo.database,
        },
        success:res=>{
          console.log(res);
          this.accountListArray = res.data.data;
        }
      })
    },
    bindPickerChange: function(e) {
      this.index = e.target.value;
      this.picker = this.array[this.index];
    },
    bindDateChange: function(e) {
      this.date = e.target.value;
      this.timer = this.date;
    },
    bindbalance: function(e) {
      this.index = e.target.value;
      this.b = this.balance[this.index];
    },
    classificationChange: function(e) {
      this.index = e.target.value;
      this.classs = this.classarray[this.index];
    },
    getDate(type) {
      const date = new Date();
      let year = date.getFullYear();
      let month = date.getMonth() + 1;
      let day = date.getDate();

      if (type === "start") {
        year = year - 60;
      } else if (type === "end") {
        year = year + 2;
      }
      month = month > 9 ? month : "0" + month;
      day = day > 9 ? day : "0" + day;
      return `${year}-${month}-${day}`;
    }
  },
  onLoad(){
    this.getAccountList();
  }
};
</script>
<style scoped>
.uni-form-item {
  display: flex;
  justify-content: center;
  position: relative;
}
.uni-list{
  display: flex;
  justify-content: center;
  flex: 1;
}
.uni-list-cell-db {
  font-size: 30upx;
  display: flex;
  align-items: center;
}
.triangle {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 12upx 8upx 0;
  border-style: solid;
  border-color: #333 transparent transparent;
  /*灰 透明 透明 */
}
.inp1 {
  width: 600upx;
  border: 1px solid #c0c0c0;
  border-radius: 50upx;
  margin: 20upx 0;
  padding: 0 20upx;
  text-align: center;
  font-size: 30upx;
}

.inp2 {
  font-size: 30upx;
}

.uni-block {
  display: flex;
  justify-content: space-around;
  text-align: center;
  height: 70upx;
}

.iconfont {
  position: absolute;
  display: flex;
  left: 25%;
  top: 30%;
  color: #c0c0c0;
  font-size: 30upx;
}

.uni-tab-bar {
  display: flex;
  justify-content: space-around;
  font-size: 30upx;
  line-height: 60upx;
  border-bottom: 1px solid #f7f7f7;
}
.list {
  width: 25%;
  text-align: center;
}
</style>
