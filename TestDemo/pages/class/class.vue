<template>
	<view> 
		<!-- 自定义状态栏 -->
		<view class="status_title">
			<image class="status_left" src="../../static/icon/back.png" mode="widthFix" @click="back()"></image>
			<view class="status_center">人脸识别考勤系统</view>
		</view>
		
		<view class="tn-padding-bottom-lg">
		  <view class="tn-flex tn-flex-row-between">
		    <view class="justify-content-item tn-margin tn-text-bold tn-text-xxl">
		      班级查看
		    </view>
		  </view>
		  
		  <view class="tn-flex tn-margin-left tn-margin-right tn-margin-top-sm">
			<view class="tn-padding-left-sm" style="width: 100%;">
			  <view class="tn-flex tn-flex-row-between tn-flex-col-between">
			    <view class="justify-content-item">
			      <text class="tn-color-cat tn-text-lg tn-text-bold">您的
					<text style="background-color: antiquewhite; margin-left: 2rpx;margin-right: 2rpx;">{{name.sub_name}}</text>
				  班级如下：</text>
			    </view>
			  </view>
			</view>
			</view>
		  </view>
		  
		  <view class="box">
		  		<view class="line"></view>
				<view class="content" v-for="(item,index) in classInfo" :key="index">
					<view class="tn-flex tn-flex-col-top tn-margin tn-cat-shadow tn-padding" style="background-color: #fafafa;" @click="toHistory(item.name)">
										<view class="tn-padding-left-sm" style="width: 100%;">
										  <view class="tn-flex tn-flex-row-between tn-flex-col-between">
										    <view class="justify-content-item">
										      <text class="tn-color-cat tn-text-lg tn-text-bold">{{item.name}}</text>
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
										      <text class="tn-padding-right-xs">#</text> 班级
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
</template>

<script>
	export default {
		data() {
			return {
				// 向考勤历史传递的参数
				name:{
					sub_name:"",
					class_name:""
				},
				
				// 这里存放班级信息 从后端传过来
				classInfo:[{
					id: 1,
					name:'软件工程211',
					detail:'班级详情',
					time:'创建时间'
				},{
					id: 2,
					name:'软件工程212',
					detail:'班级详情',
					time:'创建时间'
				}] 
			}
		},
		
		onLoad(options) {
			this.name.sub_name = decodeURIComponent(options.sub_name);
		},
		
		methods: {
			// 返回上一级
			back(){
				uni.navigateBack({
					delta: 0
				})
			},
			
			// 去考勤历史页面
			toHistory(class_name){
				this.name.class_name = class_name
				// console.log(this.name)
				uni.navigateTo({
					url:'/pages/history/history?name='+encodeURIComponent(JSON.stringify(this.name)),
				});
			}
			
		}
	}
</script>

<style lang="scss"> 
/* 自定义导航栏 */
.status_title {
	box-sizing: border-box;
	display: flex;
	justify-content: space-between;
	align-items: center;
	width: 100%;
	height: 44px;
	padding: 16px;
	background-color: #fafafa;
	
	.status_left {
		width: 18px;
	}
	.status_center {
		font-size: 17px;
		font-weight: 700;
	}
}

// 内容
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
