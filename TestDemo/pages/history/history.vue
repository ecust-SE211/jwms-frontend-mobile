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
		      考勤记录
		    </view>
		  </view>
		  
		  <view class="tn-flex tn-margin-left tn-margin-right tn-margin-top-sm">
			<view class="tn-padding-left-sm" style="width: 100%;">
			  <view class="tn-flex tn-flex-row-between tn-flex-col-between">
			    <view class="justify-content-item">
			      <text class="tn-color-cat tn-text-lg tn-text-bold">
					  <view class="top" style="margin-bottom: 5rpx;">
					  	<text style="background-color: antiquewhite;">{{name.sub_name}}</text>
					  </view>
					<text style="background-color: antiquewhite; margin-left: 2rpx;margin-right: 5rpx;">{{name.class_name}}</text>
				  考勤记录如下：</text>
			    </view>
			  </view>
			</view>
			</view>
		  </view>
		  
		  <view class="box">
		  		<view class="line"></view>
				
				<view class="content" v-for="(item,index) in history" :key="index">
					<view class="tn-flex tn-flex-col-top tn-margin tn-cat-shadow tn-padding" style="background-color: #fafafa;" @click="toStudent()">
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
										      <text class="tn-padding-right-xs">#</text> 考勤
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
				name:{
					// 最好从后端传过来
					class_name:""	,// 班级名称
					sub_name:""   // 课程名称
				},
				// 历史考勤记录需要从后端传过来
				history:[
					{
						id: 1,
						name: "考勤记录",
						detail:"考勤详情",
						time: "创建时间"
					},{
						id: 2,
						name: "考勤记录",
						detail:"考勤详情",
						time: "创建时间"
					},{
						id: 3,
						name: "考勤记录",
						detail:"考勤详情",
						time: "创建时间"
					}
				]
			};
		},
		
		onLoad(options) {
			this.name = JSON.parse(decodeURIComponent(options.name));
		    // console.log(this.name);
		},
		
		methods:{
			// 返回上一级
			back(){
				uni.navigateBack({
					delta: 1
				})
			},
			
			// 去学生页面
			toStudent(){
				uni.navigateTo({
					url:'/pages/student/student'
				})
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
