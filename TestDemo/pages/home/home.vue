<template>
  <view class="index tn-safe-area-inset-bottom">
    <!-- 顶部自定义导航 -->
    <tn-nav-bar fixed :isBack="false" :bottomShadow="false" backgroundColor="#FFFFFF" >
      <view class="custom-nav tn-flex tn-flex-col-center tn-flex-row-left">
		  
        <!-- 图标logo -->
        <view style="width: 100%;">
        	<view class="custom-nav__back" style="float: right;">
        	    <view class="tn-icon-menu" style="font-size: 50rpx;" @click="toMe"></view>
        	  </view>
        			<view style="float: right;">系统首页</view>
        	</view>
        </view>
    </tn-nav-bar>
    <view class="tn-flex tn-flex-row-between tn-flex-col-center tn-margin-top" :style="{paddingTop: vuex_custom_bar_height + 'px'}">
      <view class="justify-content-item tn-margin-left">
        <view class="tn-text-bold tn-text-lg tn-padding-bottom-xs">
          您好！欢迎使用人脸识别考勤系统！
        </view>
      </view>
    </view>
	<!-- 图片轮播器 -->
   <swiper class="card-swiper" :circular="true"
      :autoplay="true" duration="500" interval="8000" > 
      <swiper-item v-for="(item,index) in swiperList" :key="index">
        <view class="swiper-item image-banner">
          <image :src="item.url" mode="aspectFill"></image>
        </view>
      </swiper-item>
    </swiper>
	
    <view class="indication">
        <block v-for="(item,index) in swiperList" :key="index">
            <view class="spot" :class="cardCur==index?'active':''"></view>
        </block>
    </view>
    
    <view class="tn-padding-bottom-lg">
      <view class="tn-flex tn-flex-row-between">
        <view class="justify-content-item tn-margin tn-text-bold tn-text-xxl">
          课程查看
        </view>
      </view>
      
      <view class="tn-flex tn-margin-left tn-margin-right tn-margin-top-sm">
		<view class="tn-padding-left-sm" style="width: 100%;">
		  <view class="tn-flex tn-flex-row-between tn-flex-col-between">
		    <view class="justify-content-item">
		      <text class="tn-color-cat tn-text-lg tn-text-bold">{{content[0].name}}老师，您的课程如下：</text>
		    </view>
		  </view>
		</view>
      </view>
	
	<view class="box">
			<view class="line"></view>
			 <view class="content" v-for="(item,index) in content[0].subjects" :key="index">
			 	<view class="tn-flex tn-flex-col-top tn-margin tn-cat-shadow tn-padding" style="background-color: #fafafa;" @click="toClass(item.n)">
			 						<view class="tn-padding-left-sm" style="width: 100%;">
			 						  <view class="tn-flex tn-flex-row-between tn-flex-col-between">
			 						    <view class="justify-content-item">
			 						      <text class="tn-color-cat tn-text-lg tn-text-bold">{{item.n}}</text>
			 						    </view>
			 						    <view class="justify-content-item tag">
			 						      {{item.id}}
			 						    </view>
			 						  </view>
			 						  <view class=" tn-padding-top-xs  tn-text-ellipsis-2">
			 						    <text class="tn-color-gray">{{item.detail}}</text>
			 						  </view>
			 						  <view class="tn-flex tn-flex-row-between tn-flex-col-between tn-margin-top-sm">
			 						    <view class="justify-content-item tn-round tn-text-xs tn-bg-orangered--light tn-color-orangered" style="padding: 5rpx 15rpx;">
			 						      <text class="tn-padding-right-xs">#</text> 课程
			 						    </view>
			 						    <view class="justify-content-item tn-color-gray tn-text-center tn-color-gray">
			 						      <text class="tn-icon-time tn-padding-right-xs tn-text-df"></text>
			 						      <text class="tn-text-sm">{{item.time}}</text>
			 						    </view>
			 						  </view>
			 						</view>
			 	</view>
			 </view>
	</view>
      
    </view>
  
    
  </view>
</template>

<script>
  export default {
    name: 'Index',
    data(){
      return {
        isAndroid: true,
		// 轮播器的图片
        swiperList: [{
          id: 1,
          url: require('../../static/ecust1.jpg'),
        }, {
          id: 2,
          url: require('../../static/ecust2.jpg'),
        }, {
          id: 3,
          url: require('../../static/ecust3.jpg'),
        }, {
          id: 4,
          url: require('../../static/ecust4.jpg'),
        }, {
          id: 5,
          url: require('../../static/ecust5.jpg'),
        }],
		
		// 下面是课程查看 应该是从数据库传来的东西 赋值在下面
        content: [
         {
			 name:"XX",
			 subjects:[{
				 id: 1,
				 n:'数据库原理与设计',
				 detail:'课程详情',
				 time:'创建时间'
			 },{
				 id: 2,
				 n:'软件文档写作',
				 detail:'课程详情',
				 time:'创建时间'
			 }]
		 }
        ]
      }
    },
    created() {
      const systemInfo = uni.getSystemInfoSync()
      if (systemInfo.system.toLocaleLowerCase().includes('ios')) {
        this.isAndroid = false
      } else {
        this.isAndroid = true
      }
    },
    methods: {

      // 跳转class页面
      toClass(sub_name) {	
		// console.log(sub_name)
      	uni.navigateTo({
      		url: "/pages/class/class?sub_name=" +  encodeURIComponent(sub_name),
      	});
      },
	  
	  // 跳转个人中心页面
	  toMe(){
		 uni.reLaunch({
		 	url: '/pages/mine/mine'
		 });
	  }
    }
  }
</script>

<style lang="scss" scoped>

  /* 轮播视觉差 start */
  .card-swiper {
    height: 330rpx !important;
  }
    
  .card-swiper swiper-item {
    width: 750rpx !important;
    left: 0rpx;
    box-sizing: border-box;
    padding: 40rpx 30rpx 30rpx 30rpx;
    overflow: initial;
  }
    
  .card-swiper swiper-item .swiper-item {
    width: 100%;
    display: block;
    height: 100%;
    border-radius: 15rpx;
    transform: scale(1);
    transition: all 0.2s ease-in 0s;
    // overflow: hidden;
  }
    
  .card-swiper swiper-item.cur .swiper-item {
    transform: none;
    transition: all 0.2s ease-in 0s;
  }
    
  .card-swiper swiper-item .swiper-item-text {
    margin-top: -160rpx;
    text-align: center;
    width: 100%;
    display: block;
    height: 50%;
    border-radius: 10rpx;
    transform: translate(100rpx, -60rpx) scale(0.9, 0.9);
    transition: all 0.6s ease 0s;
    overflow: hidden;
  }
    
  .card-swiper swiper-item.cur .swiper-item-text {
    margin-top: -220rpx;
    width: 100%;
    transform: translate(0rpx, 0rpx) scale(0.9, 0.9);
    transition: all 0.6s ease 0s;
  }
  
  .image-banner{
    border: 1rpx solid #F8F7F8;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .image-banner image{
    width: 100%;
    height: 100%;
  }
  
  /* 轮播指示点 start*/
  .indication{
    z-index: 9999;
    width: 100%;
    height: 36rpx;
    position: absolute;
    display:flex;
    flex-direction:row;
    align-items:center;
    justify-content:center;
  }
  
  .spot{
    background-color: #FFFFFF;
    opacity: 0.6;
    width: 10rpx;
    height: 10rpx;
    border-radius: 20rpx;
    top: -70rpx;
    margin: 0 8rpx !important;
    position: relative;
  }
  
  .spot.active{
    opacity: 1;
    width: 30rpx;
    background-color: #FFFFFF;
  }
  

  
 /* 图标容器12 start */
 .tn-three{
     position: absolute;
     top: 50%;
     right: 50%;
     bottom: 50%;
     left: 50%;
     transform: translate(-38rpx, -20rpx) rotateX(20deg) rotateY(10deg) rotateZ(-20deg);
     text-shadow: -1rpx 2rpx 0 #f0f0f0, -2rpx 4rpx 0 #f0f0f0, -10rpx 20rpx 30rpx rgba(0, 0, 0, 0.2);
 }
 .icon12 {
   &__item {
     width: 30%;
     background-color: #FFFFFF;
     border-radius: 10rpx;
     padding: 30rpx;
     margin: 20rpx 10rpx;
     transform: scale(1);
     transition: transform 0.3s linear;
     transform-origin: center center;
     
     &--icon {
       width: 100rpx;
       height: 100rpx;
       font-size: 60rpx;
       border-radius: 50%;
       margin-bottom: 18rpx;
       position: relative;
       z-index: 1;
       
       &::after {
         content: " ";
         position: absolute;
         z-index: -1;
         width: 100%;
         height: 100%;
         left: 0;
         bottom: 0;
         border-radius: inherit;
         opacity: 1;
         transform: scale(1, 1);
         background-size: 100% 100%;
         background-image: url(https://resource.tuniaokj.com/images/cool_bg_image/icon_bg6.png);
 
           
       }
     }
   }
 }
  
  /* 自定义导航栏内容 start */
  .custom-nav {
    height: 100%;
    
    &__back {
      margin: auto 5rpx;
      font-size: 40rpx;
      margin-right: 10rpx;
      margin-left: 550rpx;
      flex-basis: 5%;
    }
    
    &__search {
      flex-basis: 60%;
      width: 100%;
      height: 100%;
      
      &__box {
        width: 100%;
        height: 70%;
        padding: 10rpx 0;
        margin: 0 30rpx;
        border-radius: 60rpx 60rpx 0 60rpx;
        font-size: 24rpx;
      }
      
      &__icon {
        padding-right: 10rpx;
        margin-left: 20rpx;
        font-size: 30rpx;
      }
      
      &__text {
        // color: #AAAAAA;
      }
    }
  }
  .logo-image{
    width: 65rpx;
    height: 65rpx;
    position: relative;
    border: 1rpx solid #F8F7F8;
    border-radius: 50%;
  }
  .logo-pic{
    background-size: cover;
    background-repeat:no-repeat;
    // background-attachment:fixed;
    background-position:top;
    border-radius: 50%;
  }
  /* 自定义导航栏内容 end */
  
  .box{
	  text-align: center;
	  .line{
	  	    width: 50%;
	  	    height: 1px;
	  	    border-top: solid #ACC0D8 5rpx;
	  		margin-top: 20rpx;
			margin-left: 25%;
	  }
  }
</style>