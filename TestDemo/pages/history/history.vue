<template>
	<view>
		<!-- 自定义状态栏 -->
		<view class="status_title">
			<image class="status_left" src="../../static/icon/back.png" mode="widthFix" @click="back()"></image>
			<view class="status_center">人脸识别考勤系统</view>
		</view>
		<view class="tn-padding-bottom-lg">
			<div style="padding: 15px;;">
				<span style="font-size: 1.125rem;">
					考勤记录
				</span>
				<div class="box">
					<div class="content" v-for="(item,index) in history" :key="index">
						<view class="tn-flex tn-flex-col-top tn-cat-shadow tn-padding"
							style="background-color: #fafafa;" @click="toStudent(item.id)">
							<view class="tn-padding-left-sm" style="width: 100%;">
								<view class="tn-flex tn-flex-row-between tn-flex-col-between">
									<view class="justify-content-item tag">
										考勤记录{{item.id}}
									</view>
								</view>
								<view class=" tn-padding-top-xs  tn-text-ellipsis-2">
								</view>
								<view class="tn-flex tn-flex-row-between tn-flex-col-between tn-margin-top-sm">
									<view
										class="justify-content-item tn-round tn-text-xs tn-bg-orangered--light tn-color-orangered"
										style="padding: 5rpx 15rpx;">
										<text class="tn-padding-right-xs">#</text> 考勤
									</view>
									<view class="justify-content-item tn-color-gray tn-text-center tn-color-gray">
										<text class="tn-icon-time tn-padding-right-xs tn-text-df"></text>
										<text class="tn-text-sm">{{item.date}}</text>
									</view>
								</view>
							</view>
						</view>
					</div>
				</div>
			</div>
		</view>
		<button
			style="position: fixed;bottom: 0;right: 0;width: 100%;height: 10%;color:#FFFFFF;font-size: larger;background: #00C3FF"
			@click="issueAttendance">发布考勤</button>
	</view>
</template>

<script>
	import * as requestUtil from "@/utils/request";
	export default {
		data() {
			return {
				name: {
					// 最好从后端传过来
					class_id: "", // 班级名称
					sub_name: "" // 课程名称
				},
				// 历史考勤记录需要从后端传过来
				history: []
			};
		},

		onLoad(options) {
			this.name = JSON.parse(decodeURIComponent(options.name));
			this.initHistory();
		},

		methods: {
			async issueAttendance() {
				const res = await requestUtil.post("attendance", {
					id: '',
					clid: this.name.class_id,
					date: ''
				});
				if (res.data.code === '200') {
					alert("发布考勤成功");
					// 刷新页面
					window.location.reload();
				}
			},
			async initHistory() {
				const res = await requestUtil.get("attendance/findAttendance", {
					clid: this.name.class_id
				});
				// console.log(res);
				if (res.data.code === '200') {
					this.history = res.data.data.records;
				}
			},
			// 返回上一级
			back() {
				uni.navigateBack({
					delta: 1
				})
			},

			// 去学生页面
			toStudent(fid) {
				uni.navigateTo({
					url: '/pages/student/student?fid=' + encodeURIComponent(JSON.stringify({
						fid: fid,
						clid: this.name.class_id
					}))
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

	.box {
		text-align: center;

		.line {
			width: 50%;
			height: 1px;
			border-top: solid #ACC0D8 5rpx;
			margin-top: 20rpx;
			margin-left: 25%;
		}
	}
</style>
