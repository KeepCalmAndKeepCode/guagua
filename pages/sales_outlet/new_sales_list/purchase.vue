<template>
  <view class>
    <form class="form" @submit="saveskipSelectClient">
      <view class="section">
        <text class>客户证件号</text>
        <input
          class="uni-input"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="newPassWord"
          placeholder="填写证件号码"
        >
      </view>
      <view class="section">
        <text class>客户名称</text>
        <input
          class="uni-input"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="newPassWord"
          placeholder="填写供应商名称"
        >
      </view>
      <view class="section" @click="togglePopup('bottom-share')">
        <text class>客户类别</text>
        <input
          class="uni-input"
          placeholder-style="color:#CCCCCC;font-size:30upx"
          name="newPassWord"
          placeholder="选择供应商类别"
          disabled="true"
        >
        <view class="triangle"></view>
      </view>
      <view class="uni-btn-v save">
        <button class="formSub" formType="submit">保存，下一步</button>
      </view>
      <view class="uni-btn-v jump" @click="skipSelectClient">
        <button class="formSub" formType>无需新增，跳过</button>
      </view>
    </form>
    <view v-show="typeMid" class="middlePopup" >
      <view class="containerMiddle">
        <view class="title">添加供应商类别</view>
        <input class="uni-input className" type="text" placeholder="填写新的类别名称">
        <view class="control">
          <view @click="closeMid()">取消</view>
          <view @click="">保存</view>
        </view>
      </view>
		</view>
    <uni-popup :show="type === 'bottom-share'" position="bottom" mode="bottom" @hidePopup="togglePopup('')">
        <view class="classes add" @click="popupMid()">+ 添加类别</view>
        <view class="classes">默认供应商</view>
        <view class="classes">默认供应商2</view>
		</uni-popup>
  </view>
</template>

<script>
import uniPopup from '../../components/uni-popup';
export default {
  components: {
    uniPopup
  },
  data(){
    return{
      type: '',
      typeMid: false,
    }
  },
  methods: {
    togglePopup(type){
      this.type = type;
      this.typeMid = false;
    },
    popupMid(){
      this.typeMid = true;
    },
    closeMid(){
      this.typeMid = false;
    },
    saveskipSelectClient(){
      uni.navigateTo({
        url: "./choice"
      });
    },
    skipSelectClient(){
      uni.navigateTo({
        url: "./choice"
      });
    }
  }
};
</script>

<style scope>
.section {
  display: flex;
  align-items: center;
  height: 90upx;
  border-bottom: #dddddd 1px solid;
  font-size: 15px;
}
.section > text {
  width: 260upx;
  text-align: center;
}
.triangle {
  margin-left: 20upx;
  width: 0;
  height: 0;
  border-width: 12upx 8upx 0;
  border-style: solid;
  border-color: #333 transparent transparent; /*灰 透明 透明 */
}
.save .formSub ,.jump .formSub{
  color: #fff;
  background-color: #136cae;
  font-size: 30upx;
  margin: 20upx;
}
.jump .formSub{
  background-color: #8D8D8D;
}
.classes{
  border-bottom: 1px solid #BBBBBB; 
  height: 88upx;
  font-size: 30upx;
}
.add{
  color: #136cae;
}
.containerMiddle {
  width: 100%;
}
.containerMiddle>view{
  width: 100%;
}
.containerMiddle .title{
  font-weight: 600;
  text-align: center;
  height: 100upx;
  line-height: 100upx;
}
.className{
  background-color: #fff;
  width: 85%;
  margin: 0 auto 40upx;
  border: solid 1px #BBBBBB;
}
.middlePopup{
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-100%);
  background-color: #DFDFDF;
  z-index: 999;
  width: 90%;
  font-size: 30upx;
}
.control{
  border-top:1px solid #BBBBBB;
  display: flex;
  align-items: center;
  height: 80upx;
}
.control>view{
  width: 50%;
  text-align: center;
  line-height: 80upx;
}
.control :nth-child(2){
  color: #136cae;
  border-left: 1px solid #BBBBBB;
}
</style>
