
<template>
  <view class="content">
    <view class="shade" v-show="shade" @click="openShade"></view>
    <view class="search">
      <text class="iconfont iconfangdajing"></text>
      <input
        class="uni-input"
        placeholder-style="color:#888888"
        name="keyword"
        placeholder="键入关键字查询"
        @input="queryGoods"
      >
    </view>
    <view class="findNum">找到1个商品</view>
    <view class="tab">
      <view class @click="changeType1">
        {{tab1[controlType1.select].name}}
        <view class="triangle"></view>

        <view class="select" v-show="controlType1.show">
          <view
            :class="{selectType : controlType1.select == index}"
            @click="select1(index)"
            v-for="(item,index) in tab1"
            :key="item.id"
          >{{ item.name }}</view>
        </view>
      </view>
      <view class @click="changeType2">
        添加时间({{tab2[controlType2.select]}})
        <view class="triangle"></view>

        <view class="select" v-show="controlType2.show">
          <view
            :class="{selectType : controlType2.select == index}"
            @click="select2(index)"
            v-for="(item,index) in tab2"
            :key="item.id"
          >{{ item }}</view>
        </view>
      </view>
      <view class @click="changeType3">
        {{tab3[controlType3.select]}}
        <view class="triangle"></view>

        <view class="select" v-show="controlType3.show">
          <view
            :class="{selectType : controlType3.select == index}"
            @click="select3(index)"
            v-for="(item,index) in tab3"
            :key="item.id"
          >{{ item }}</view>
        </view>
      </view>
    </view>
    <view class="main" v-for="(item,index) in list" :key="item.id" @click="skipNewGoods(index)">
      <view class="itemOne">
        <text class="one">{{item.name}}</text>
        <text class="price">￥{{item.salePrice}}</text>
      </view>
      <view class="itemTwo">
        <text class="one">{{item.number}}</text>
        <view class>
          <text>当前仓库</text>
          <text class="num">{{item.locationName}}</text>
        </view>
      </view>
      <view class="itemThree" data-position="bottom">
        <text class="one">{{item.categoryName}}</text>
        <view v-on:click.stop="creatQrcode('bottom-share',item.name,item.barCode)">
          <text class="printQrcode">打印二维码</text>
          <text class="iconfont icondayinjidayinxianxing"></text>
        </view>
      </view>
    </view>
    <view class="add" v-show="shade">
      <view class="triangleAdd"></view>
      <view class="addContent bor">
        <text class="iconfont iconsaoyisaoerweimasaomasaomiao"></text>
        <text>扫码添加</text>
      </view>
      <view class="addContent" @click="skipNewGoods('')">
        <text class="iconfont iconxinzeng"></text>
        <text>手动填写</text>
      </view>
    </view>
    <uni-popup
      :show="type === 'bottom-share'"
      position="bottom"
      mode="bottom"
      @hidePopup="togglePopup('')"
    >
      <view class>{{ productName }}</view>
      <qrcode :val="qrval" :size="qrsize" ref="qrcode"></qrcode>
      <view class="uni-center center-box print">打印</view>
      <view class="uni-center center-box" @click="togglePopup('')">取消</view>
    </uni-popup>
  </view>
</template>

<script>
import uniPopup from "../components/uni-popup";
import qrcode from "../components/qrcode/qrcode.vue";
import { Category, Goods } from "../../interfaces.js";
var timer;
export default {
  components: {
    uniPopup,
    qrcode
  },
  data() {
    return {
      userinfo: uni.getStorageSync("userinfo"),
      type: "",
      productName: "",
      controlType1: {
        show: false,
        select: 0
      },
      controlType2: {
        show: false,
        select: 0
      },
      controlType3: {
        show: false,
        select: 0
      },
      tab1: [{
        name:"全部"
      }],
      tab2: ["升序", "降序"],
      tab3: ["零售价", "预计采购价"],

      list: [
      ],
      qrval: "",
      qrsize: 200,
      shade: false
    };
  },
  onNavigationBarButtonTap(e) {
    this.shade = true;
  },
  methods: {
    // 获取分类
    getCategory() {
      uni.request({
        method: "GET",
        url: Category,
        data: {
          database: this.userinfo.database,
          type: "category"
        },
        success: res => {
          this.tab1.push(...res.data);
        }
      });
    },
    // 获取商品列表与商品查询
    getGoodsIndex() {
      uni.request({
        url: Goods,
        data: {
          database: this.userinfo.database
          // type: "category"
        },
        success: res => {
          this.list = res.data.data;
        }
      });
    },
    changeType1() {
      this.controlType1.show = !this.controlType1.show;
    },
    changeType2() {
      this.controlType2.show = !this.controlType2.show;
    },
    changeType3() {
      this.controlType3.show = !this.controlType3.show;
    },
    select1(index) {
      this.controlType1.select = index;
    },
    select2(index) {
      this.controlType2.select = index;
    },
    select3(index) {
      this.controlType3.select = index;
    },
    openShade() {
      this.shade = false;
    },
    skipNewGoods(index) {
      let message = JSON.stringify(this.list[index]);
      uni.navigateTo({
        url: "newGoods?commodityInfo=" + message
      });
    },
    togglePopup(type) {
      this.type = type;
    },
    creatQrcode(type, productName, qrval) {
      this.productName = productName;
      this.qrval = qrval;
      this.type = type;
      var that = this;
      var t ;
      function interval(){
        that.$refs.qrcode.creatQrcode();
        t = setTimeout(interval,100);
        if(that.$refs.qrcode.img.length != 0){
          clearInterval(t);
        }
      }
      interval();
    },
    /**
     * 超过这个时间在调用接口。
     * 超过这个时间在执行下边的代码
     */
    queryGoods(e){
      clearTimeout(timer);
      timer = setTimeout( res => {
        uni.request({
          url: Goods,
          data: {
            database: this.userinfo.database,
            skey: e.detail.value
          },
          success: res => {
            this.list = res.data.data;
          }
        });
      },1000);
    }
  },
  onLoad() {
    this.getCategory();
    this.getGoodsIndex();
  }
};
</script>

<style scope>
.search {
  position: relative;
  padding: 30upx;
}
.search .uni-input {
  background: #f2f2f2;
  font-size: 30upx;
  height: 81upx;
  line-height: 81upx;
  padding-left: 80upx;
  border-radius: 50upx;
}
.search .iconfangdajing {
  position: absolute;
  z-index: 10;
  top: 50%;
  transform: translate(30upx, -50%);
}
.findNum {
  font-size: 30upx;
  font-weight: 600;
  padding-left: 30upx;
}
.tab {
  font-size: 30upx;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #f0f0f0;
  padding: 20upx 0;
}
.triangle {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 12upx 8upx 0;
  border-style: solid;
  border-color: #333 transparent transparent; /*灰 透明 透明 */
}

.tab > view {
  display: flex;
  align-items: center;
  flex: 1;
  justify-content: center;
  position: relative;
}
.select {
  position: absolute;
  top: 50upx;
  border: solid 1px #3782ba;
  background-color: #fff;
}
.select > view {
  padding: 5upx 30upx;
}
.selectType {
  background: #3782ba;
  color: #fff;
}
.main {
  border-bottom: 1px solid #f0f0f0;
  padding: 20upx 40upx;
}
.main > view {
  display: flex;
  align-items: center;
}
.main > view > .one {
  flex: 1;
}
.main .price {
  color: #009587;
  font-weight: 600;
}
.main .num {
  color: #ff9800;
  margin-left: 40upx;
}
.main > .itemOne {
  font-size: 32upx;
}
.main > .itemTwo > view {
  font-size: 24upx;
}
.main > .itemThree {
  font-size: 24upx;
  color: #666666;
}
.main > .itemThree > view {
  display: flex;
  align-items: center;
}
.main > .itemThree > view > .printQrcode {
  color: #2c84cb;
  margin-right: 10upx;
}
.icondayinjidayinxianxing {
  font-size: 30upx;
}
.add {
  background-color: #444444;
  width: 288upx;
  position: fixed;
  top: 88upx;
  right: 12upx;
  z-index: 999;
  color: #fff;
}
page {
  width: 100%;
  height: 100%;
}
.content {
  position: relative;
  width: 100%;
  height: 100%;
}
.triangleAdd {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 0 14upx 20upx;
  border-style: solid;
  border-color: transparent transparent #444444;
  position: absolute;
  top: -20upx;
  left: 226upx;
  z-index: 999;
}
.addContent {
  display: flex;
  align-items: center;
  height: 80upx;
  width: 100%;
  justify-content: center;
  font-size: 28upx;
}
.addContent > .iconfont {
  margin-right: 20upx;
}
.add .bor {
  border-bottom: solid 1px #fff;
}
.shade {
  position: fixed;
  width: 100%;
  height: 100%;
  z-index: 999;
}
.print {
  color: #2c84cb;
  border-top: #bbbbbb 1px solid;
  border-bottom: #bbbbbb 1px solid;
}
.qrcode {
  margin: 0 auto;
}
</style>
