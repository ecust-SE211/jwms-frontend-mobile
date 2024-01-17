<template>
	<view>
		<view class="status_title">
			<image class="status_left" src="../../static/icon/back.png" mode="widthFix" @click="back()"></image>
			<view class="status_center">人脸识别考勤系统</view>
		</view>

		<view class="tn-padding-bottom-lg">
			<div style="padding: 15px;;">
				<span style="font-size: 1.125rem;">
					班级查看
				</span>
				<div class="box">
					<div class="content" v-for="(item,index) in classInfo" :key="index">
						<view class="tn-flex tn-flex-col-top tn-cat-shadow tn-padding" @click="toHistory(item.id)">
							<view class="tn-padding-left-sm" style="width: 100%;">
								<view class="tn-flex tn-flex-row-between tn-flex-col-between">
									<view class="justify-content-item tag">
										班级:S{{item.id}}
									</view>
								</view>
								<view class="tn-flex tn-flex-row-between tn-flex-col-between tn-margin-top-sm">
									<view
										class="justify-content-item tn-round tn-text-xs tn-bg-orangered--light tn-color-orangered"
										style="padding: 5rpx 15rpx;">
										<text class="tn-padding-right-xs">#</text> 班级
									</view>
									<view class="justify-content-item tn-color-gray tn-text-center tn-color-gray">
										<text class="tn-icon-time tn-padding-right-xs tn-text-df"></text>
									</view>
								</view>
							</view>
						</view>
					</div>
				</div>
			</div>
		</view>
	</view>
</template>

<script>
	import requestUtil from "@/utils/request";

	export default {
		data() {
			return {
				teacherID: localStorage.getItem("teacher"),
				// 向考勤历史传递的参数
				sub_id: '',
				name: {
					sub_name: "",
					class_id: ""
				},
				// 这里存放班级信息 从后端传过来
				classInfo: []
			}
		},

		onLoad(options) {
			this.sub_id = decodeURIComponent(options.sub_id);
			this.initClass();
			this.initSubName();
		},

		methods: {
			async initSubName() {
				const res = await requestUtil.get("course/" + this.sub_id, {
					id: this.sub_id
				});
				if (res.data.code === '200') {
					this.name.sub_name = res.data.data.name;
				}
			},
			async initClass() {
				const res = await requestUtil.get("teacherrole/page", {
					pageNum: 1,
					pageSize: 10
				})
				if (res.data.code === '200') {
					for (const item of Array.from(res.data.data.records)) {
						const resclass = await requestUtil.get("class/" + item.clid, {
							id: item.clid
						})
						if (resclass.data.code === '200') {
							this.classInfo.push({
								id: resclass.data.data.id
							});
						}
					}
				}
			},
			// 返回上一级
			back() {
				uni.navigateBack({
					delta: 1
				})
			},

			// 去考勤历史页面
			toHistory(class_id) {
				this.name.class_id = class_id
				// console.log(this.name)
				uni.navigateTo({
					url: '/pages/history/history?name=' + encodeURIComponent(JSON.stringify(this.name)),
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

	.content {
		display: block;
		border-radius: 0.25rem;
		box-shadow: 0 0 1rem #d0d0d7;
		margin-top: 1rem;
		font-size: 1rem;
	}
</style>
