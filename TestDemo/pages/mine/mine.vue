<template>
  <view class="mine tn-safe-area-inset-bottom">

    <!-- 顶部自定义导航 -->
    <tn-nav-bar :isBack="false" :bottomShadow="false" backgroundColor="none">
      <view class="custom-nav tn-flex tn-flex-col-center tn-flex-row-left" @click="tn('/pages/home/home')">
        <view class="custom-nav__back" style="font-size: 30rpx;">
          返回
        </view>
      </view>
    </tn-nav-bar>
    
    <view class="top-backgroup">
      <image src='https://resource.tuniaokj.com/images/my/my-bg4.png' mode='widthFix' class='backgroud-image'></image>
    </view>
    
    <view class="about__wrap" :style="{paddingTop: vuex_custom_bar_height + 'px'}">
      <!-- 图标logo/头像 -->
      <view class="tn-flex tn-flex-row-between tn-flex-col-center tn-margin-bottom" style="margin-top: -450rpx;">
        <view class="justify-content-item">
          <view class="tn-flex tn-flex-col-center tn-flex-row-left">
            <view class="logo-pic tn-shadow">
              <view class="logo-image">
                <view class="tn-shadow-blur" style="background-image:url('https://cdn.nlark.com/yuque/0/2022/jpeg/280373/1664005699053-assets/web-upload/8645ea3a-e0a9-4422-8364-cc5ede305c9f.jpeg');width: 110rpx;height: 110rpx;background-size: cover;">
                </view>
              </view>
            </view>
            <view class="tn-padding-right">
              <view class="tn-padding-right tn-padding-left-sm">
                <text class="tn-color-cat tn-text-xl tn-text-bold">张小勤</text>
                <!-- <text class=" tn-round tn-text-xs tn-bg-red tn-color-white tn-margin-left-sm" style="padding: 10rpx 20rpx;" @click="tn('/minePages/realname')">
                  未实名
                </text> -->
              </view>
              <view class="tn-padding-right tn-padding-top-xs tn-padding-left-sm tn-text-ellipsis">
                <!-- <text class="tn-color-gray">高级UI设计</text> -->
                <text class="tn-color-gray tn-padding-left-sm tn-text-sm">139****1193</text>
              </view>
            </view>

          </view>
        </view>
        <view class="justify-content-item"  @click="tn('/minePages/set')">
          <view class="tn-icon-right tn-color-gray">
          </view>
        </view>
      </view>
      
    
      <view class="tn-flex tn-flex-row-between tn-bg-white about-shadow tn-margin-top-xl">
        <view class="tn-padding-sm tn-margin-xs" @click="tn('/discoveryPages/edit')">
          <view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center tn-margin-left">
            <view class="icon15__item--icon tn-flex tn-flex-row-center tn-flex-col-center tn-shadow-blur" style="background-color: #F3F2F7;color: #7C8191;">
              <view class="tn-icon-discover"></view>
            </view>  
            <view class="tn-text-center">
              <text class="tn-text-ellipsis">我的课程</text>
            </view>
          </view>
        </view>
        <view class="tn-padding-sm tn-margin-xs" @click="tn('/circlePages/circle')">
          <view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center">
            <view class="icon15__item--icon tn-flex tn-flex-row-center tn-flex-col-center tn-shadow-blur" style="background-color: #F3F2F7;color: #7C8191;">
              <view class="tn-icon-topics"></view>
            </view>  
            <view class="tn-text-center">
              <text class="tn-text-ellipsis">签到管理</text>
            </view>
          </view>
        </view>
         <view class="tn-padding-sm tn-margin-xs" >
          <view class="tn-flex tn-flex-direction-column tn-flex-row-center tn-flex-col-center tn-margin-right">
            <view class="icon15__item--icon tn-flex tn-flex-row-center tn-flex-col-center tn-shadow-blur" style="background-color: #F3F2F7;color: #7C8191;">
              <view class="tn-icon-money"></view>
            </view>  
            <view class="tn-text-center">
              <text class="tn-text-ellipsis">云盘</text>
            </view>
          </view>
        </view>
      </view> 
     
	 
      <view class="about-shadow tn-margin-top-lg tn-padding-top-sm tn-padding-bottom-sm">
        <tn-list-cell :hover="true" :unlined="true" :radius="true" :fontSize="30"@click="tn('/minePages/about')">
          <view class="tn-flex tn-flex-col-center">
            <view
              class="icon1__item--icon tn-flex tn-flex-row-center tn-flex-col-center" style="color: #7C8191;">
              <view class="tn-icon-science-fill"></view>
            </view>
            <view class="tn-margin-left-sm tn-flex-1">关于我们</view>
            <view class="tn-color-gray tn-icon-right"></view>
          </view>
        </tn-list-cell>
		<tn-list-cell :hover="true" :unlined="true" :radius="true" :fontSize="30">
		  <button class="tn-flex tn-flex-col-center tn-button--clear-style" open-type="feedback">
		    <view
		      class="icon1__item--icon tn-flex tn-flex-row-center tn-flex-col-center" style="color: #7C8191;">
		      <view class="tn-icon-message-fill"></view>
		    </view>
		    <view class="tn-flex tn-flex-row-between" style="width: 100%;">
		      <view class="tn-margin-left-sm">问题反馈</view>
		      <view class="tn-color-gray tn-icon-right"></view>
		    </view>
		  </button>
		</tn-list-cell>
     
        <tn-list-cell :hover="true" :unlined="true" :radius="true" :fontSize="30" @click="tn('/minePages/protocol')">
          <view class="tn-flex tn-flex-col-center">
            <view
              class="icon1__item--icon tn-flex tn-flex-row-center tn-flex-col-center" style="color: #7C8191;">
              <view class="tn-icon-safe-fill"></view>
            </view>
            <view class="tn-margin-left-sm tn-flex-1" @click="goback">设置管理</view>
            <view class="tn-color-gray tn-icon-right"></view>
          </view>
        </tn-list-cell>
      </view>

    </view>
	
  </view>
</template>

<script>
  export default {
    name: 'Mine',
    data() {
      return {
      }
    },
    methods: {
     
      // 跳转
      tn(e) {
        uni.navigateTo({
          url: e,
        });
      },
     
    }
  }
</script>

<style lang="scss" scoped>
  .mine{
    max-height: 100vh;
  }
  /* 底部安全边距 start*/
  .tn-tabbar-height {
  	min-height: 120rpx;
  	height: calc(140rpx + env(safe-area-inset-bottom) / 2);
    height: calc(140rpx + constant(safe-area-inset-bottom));
  }
  
  .tn-color-cat{
    color: #1D2541;
  }
  .tn-bg-cat{
    background-color: #1D2541;
  }

  
  /* 自定义导航栏内容 start */
  .custom-nav {
    height: 100%;
    
    &__back {
      margin: auto 5rpx;
      font-size: 40rpx;
      margin-right: 10rpx;
      flex-basis: 5%;
      width: 100rpx;
      position: absolute;
    }
  }
  /* 自定义导航栏内容 end */


  /* 顶部背景图 end */
  

  /* 用户头像 start */
  .logo-image {
    width: 110rpx;
    height: 110rpx;
    position: relative;
    overflow: hidden;
    border-radius: 50%;
  }

  .logo-pic {
    background-size: cover;
    background-repeat: no-repeat;
    // background-attachment:fixed;
    background-position: top;
    border: 8rpx solid rgba(255,255,255,0.05);
    box-shadow: 0rpx 0rpx 80rpx 0rpx rgba(0, 0, 0, 0.15);
    border-radius: 50%;
    overflow: hidden;
    // background-color: #FFFFFF;
  }

  /* 页面 start*/
  .about-shadow {
    border-radius: 15rpx;
    box-shadow: 0rpx 0rpx 50rpx 0rpx rgba(0, 0, 0, 0.07);
  }

  .about {

    &__wrap {
      position: relative;
      z-index: 1;
      margin: 20rpx 30rpx;
    }
  }

  /* 页面 end*/
  
  /* 图标容器15 start */
  .icon15 {
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
  
            
        }
      }
    }
  }
  
  /* 图标容器12 start */
  .tn-three{
      position: absolute;
      top: 50%;
      right: 50%;
      bottom: 50%;
      left: 50%;
      transform: translate(-38rpx, -16rpx) rotateX(30deg) rotateY(20deg) rotateZ(-30deg);
      text-shadow: -1rpx 2rpx 0 #f0f0f0, -2rpx 4rpx 0 #f0f0f0, -10rpx 20rpx 30rpx rgba(0, 0, 0, 0.2);
  }
  .icon20 {
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
          background-image: url(https://resource.tuniaokj.com/images/cool_bg_image/icon_bg.png);
        }
      }
    }
  }
  


  .button-vip {
    width: 100%;
    height: 150rpx;
    border-radius: 15rpx;
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
      background-image: url(https://resource.tuniaokj.com/images/cool_bg_image/icon_bg.png);
    }    
  }
  

  /* 图标容器12 start */
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
        width: 15rpx;
        height: 15rpx;
        font-size: 50rpx;
        border-radius: 50%;
        margin-bottom: 38rpx;
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
            
        }
      }
    }
  }
  
  /* 图标容器1 start */
  .icon1 {
    &__item {
      // width: 30%;
      background-color: #FFFFFF;
      border-radius: 10rpx;
      padding: 30rpx;
      margin: 20rpx 10rpx;
      transform: scale(1);
      transition: transform 0.3s linear;
      transform-origin: center center;
  
      &--icon {
        width: 40rpx;
        height: 40rpx;
        font-size: 40rpx;
        border-radius: 50%;
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
          background-image: url(https://resource.tuniaokj.com/images/cool_bg_image/icon_bg.png);
        }
      }
    }
  }
  
  /* 图标容器1 end */
  
  
  /* 顶部背景图 start */
  .top-backgroup {
    height: 450rpx;
    z-index: -1;
  
    .backgroud-image {
      width: 100%;
      height: 450rpx;
      // z-index: -1;
    }
  }
  
  /* 顶部背景图 end */


</style>

