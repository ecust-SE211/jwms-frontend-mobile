<template>
	<view>
		<!-- 自定义状态栏 -->
		<view class="status_title">
			<image class="status_left" src="../../static/icon/back.png" mode="widthFix" @click="back()"></image>
			<view class="status_center">人脸识别考勤系统</view>
		</view>
		<view style="display: flex; flex-direction: column;align-items: center;padding: 15px;">
			<video id="video" width="640" height="480" hidden="hidden" style="object-fit: fill;"></video>
			<canvas id="canvas" style="width: calc(100vw - 30px);height: calc(75vw - 23px);"></canvas>
			<div v-if="student!==null">{{student.name}}正在签到</div>
			<div
				style="font-size: 1.125rem;font-weight: bold;width: calc(100vw - 30px);line-height: 2rem;height: 2rem;border-bottom: 1px solid #d0d0d7;">
				学生列表:</div>
			<div
				style="width: calc(100vw - 30px); display: flex; flex-direction: column; justify-content: space-around; ">
				<div class="student" style="border-bottom: 1px solid #d0d0d7;">
					<span>学生姓名</span>
					<span>签到情况</span>
				</div>
				<div class="student" v-for="(item,index) in unattend_students" :key="index"
					@click="student = item">
					<span>{{item.name}}</span>
					<span>未签到</span>
				</div>
				<div class="student" v-for="(item,index) in attend_students" :key="index">
					<span>{{item.name}}</span>
					<span>已签到</span>
				</div>
			</div>
		</view>
	</view>
</template>

<script>
	import * as requestUtil from "@/utils/request";
	import axios from "axios";

	export default {
		data() {
			return {
				student: null,
				processing: false,
				fid: '',
				class_id: '',
				students: [], // 学生姓名 从后端获取
				attend_students: [],
				unattend_students: [],
				faceApiLoaded: false,
				catchFace: null,
				refreshCanvas: null
			};
		},
		onLoad(options) {
			this.fid = JSON.parse(decodeURIComponent(options.fid)).fid;
			this.class_id = JSON.parse(decodeURIComponent(options.fid)).clid;
			this.initStudent().then(e => {
				this.initAttendance();
			});
		},
		mounted() {
			const faceapiminScript = document.createElement('script');
			faceapiminScript.src = '/static/js/face-api.js';
			faceapiminScript.onload = async () => {
				this.faceApiLoaded = true;
				try {
					await Promise.all([
						faceapi.nets.ssdMobilenetv1.loadFromUri(
							'../../static/models/ssd_mobilenetv1_model-weights_manifest.json'),
						faceapi.nets.faceLandmark68Net.loadFromUri(
							'../../static/models/face_landmark_68_model-weights_manifest.json'),
						faceapi.nets.faceRecognitionNet.loadFromUri(
							'../../static/models/face_recognition_model-weights_manifest.json'),
					]);
					this.startVideoStream();
				} catch (e) {
					console.error('Failed to load face-api.js models:', e);
				}
			};
			document.body.appendChild(faceapiminScript);
		},
		methods: {
			startVideoStream() {
				if (!this.faceApiLoaded) return;
				const video = document.querySelector('video');
				const canvas = document.querySelector('canvas');
				const ctx = canvas.getContext('2d');
				canvas.width = video.width;
				canvas.height = video.height;
				navigator.mediaDevices.getUserMedia({
						video: true,
						audio: false
					})
					.then(function(stream) {
						video.srcObject = stream;
						video.play();
						this.detectFacesInStream(video, canvas, ctx);
					}.bind(this))
					.catch(e => console.log(e.message));
			},
			detectFacesInStream(video, canvas, ctx) {
				this.refreshCanvas = setInterval(() => {
					ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
				}, 50)
				this.catchFace = setInterval(() => {
					if (this.processing) return
					if (this.unattend_students.length === 0) {
						clearInterval(this.catchFace)
					};
					faceapi.detectSingleFace(canvas).withFaceLandmarks().withFaceDescriptor().then(result => {
						// 对检测到的人脸进行处理
						// 在人脸上画框
						ctx.strokeStyle = 'red';
						if (result) {
							this.processing = true
							const face = result;
							if(this.student!== null ){
								if(this.checkFace(face,canvas)){
									this.student=null;
								}
							}
							// const box = face.detection.box;
							// ctx.strokeRect(box.x, box.y, box.width, box.height);
							// ctx.stroke();
							// 人脸检验
							// this.checkFace(face,canvas).then(r => {
							//   if(r){
							//   }
							// })
							// try {
							// 	const box = face.detection.box;
							// 	this.cameraShoot(canvas, box, box.width, box.height).toBlob(async (
							// 		blob) => {
							// 		// 把this.unattend_students的id赋值给一个整型数组
							// 		const ids = [];
							// 		for (let i = 0; i < this.unattend_students.length; i++) {
							// 			ids.push(this.unattend_students[i].id);
							// 		}
							// 		const jsonString = JSON.stringify(ids);
							// 		// 将image转化成file文件
							// 		const formData = new FormData();
							// 		// 将文件添加到FormData对象中
							// 		formData.append('studentIdList', jsonString);
							// 		formData.append('file', blob, 'image.png');
							// 		await axios.post(
							// 			"http://localhost:8088/student/compareCircle",
							// 			formData, {
							// 				headers: {
							// 					'Content-Type': 'multipart/form-data'
							// 				}
							// 			}).then(async res => {
							// 			if (res.data.code === '200') {
							// 				if (res.data.data !== false) {
							// 					try {
							// 						const res2 = await requestUtil
							// 							.post("attendancerecord", {
							// 								id: '',
							// 								sid: res.data.data,
							// 								fid: this.fid,
							// 							});
							// 						if (res2.data.code === '200') {
							// 							alert("签到成功!")
							// 							this.initAttendance()
							// 						}
							// 					} catch (e) {}
							// 				}
							// 			}
							// 		});
							// 	})
							// } catch (e) {}
							this.processing = false
						}
					})
				}, 1000); // 每隔一定时间检测一次
			},
			cameraShoot(video, startPoint, width, height) {
				const canvas = document.createElement("canvas");
				canvas.width = video.width;
				canvas.height = video.height;
				canvas.getContext("2d").drawImage(video, startPoint.x, startPoint.y, width, height, 0, 0, canvas.width,
					canvas.height)
				return canvas;
			},
			// checkFace(face, canvas) {
			// 	const control = document.createElement("img")
			// 	const url = '../../static/faces/'
			// 	for (let student of this.unattend_students) {
			// 		control.src = url + student.id + '.jpg';
			// 		const controlface = faceapi.detectSingleFace(control).withFaceLandmarks().withFaceDescriptor();
			// 		if (face.descriptor) {
			// 			const faceDistance = faceapi.euclideanDistance(controlface.descriptor, face.descriptor);
			// 			if (faceDistance < 0.4) {
			// 				try {
			// 					const box = face.detection.box;
			// 					this.cameraShoot(canvas, box, box.width, box.height).toBlob((blob) => {
			// 						// 将image转化成file文件
			// 						const formData = new FormData();
			// 						// 将文件添加到FormData对象中
			// 						formData.append('studentId', student.id);
			// 						formData.append('file', blob, 'image.png');
			// 						axios.post("http://localhost:8088/student/compare", formData, {
			// 							headers: {
			// 								'Content-Type': 'multipart/form-data'
			// 							}
			// 						}).then(async res => {
			// 							if (res.data.code === '200') {
			// 								if (res.data.data) {
			// 									try {
			// 										const res2 = await requestUtil.post(
			// 											"attendancerecord", {
			// 												id: '',
			// 												sid: student.id,
			// 												fid: this.fid,
			// 											});
			// 										if (res2.data.code === '200') {
			// 											alert(student.name + "签到成功");
			// 											this.initAttendance()
			// 										}
			// 									} catch (e) {}
			// 								}
			// 							}
			// 						});
			// 					})
			// 				} catch (e) {}
			// 				return true;
			// 			} else {
			// 				return false;
			// 			}
			// 		} else {
			// 			return false;
			// 		}
			// 	}
			// },
			checkFace(face, canvas) {
				try {
					const box = face.detection.box;
					this.cameraShoot(canvas, box, box.width, box.height).toBlob((blob) => {
						// 将image转化成file文件
						const formData = new FormData();
						// 将文件添加到FormData对象中
						formData.append('studentId', this.student.id);
						formData.append('file', blob, 'image.png');
						axios.post("http://localhost:8088/student/compare", formData, {
							headers: {
								'Content-Type': 'multipart/form-data'
							}
						}).then(async res => {
							if (res.data.code === '200') {
								console.log(res.data.data)
								if (res.data.data) {
									try {
										const res2 = await requestUtil.post(
											"attendancerecord", {
												id: '',
												sid: this.student.id,
												fid: this.fid,
											});
										if (res2.data.code === '200') {
											alert(this.student.name + "签到成功");
											this.initAttendance()
											return true;
										}
										else return false;
									} catch (e) {}
								}
							}
						});
					})
				} catch (e) {}
			},
			async initAttendance() {
				const res = await requestUtil.get("attendancerecord/student", {
					fid: this.fid
				});
				if (res.data.code === '200') {
					this.attend_students = []
					this.unattend_students = []
					const rec = res.data.data.records;
					for (let student of this.students) {
						this.find(rec, student.id).then(r => {
							if (r) {
								this.attend_students.push({
									id: student.id,
									name: student.name
								})
							} else {
								this.unattend_students.push({
									id: student.id,
									name: student.name
								})
							}
						})
					}
				}
			},
			async find(rec, id) {
				for (let item of rec) {
					if (item.sid === id) {
						return true;
					}
				}
				return false;
			},
			async initStudent() {
				const res = await requestUtil.get("classstudent/student", {
					clid: this.class_id
				});
				if (res.data.code === '200') {
					this.students = res.data.data;
				}
			},
			// 返回上一级
			back() {
				clearInterval(this.refreshCanvas)
				clearInterval(this.catchFace)
				window.history.back()
				// uni.navigateBack({
				// 	delta: 1
				// })
			},
		},

	}
</script>

<style lang="scss">
	/* 页面 start*/
	.student {
		width: calc(100vw - 30px);
		height: 2rem;
		align-items: center;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
	}

	.student>span {
		width: calc(50vw - 15px);
		font-size: 1rem;
		font-weight: bold;
		display: block;
		text-align: center;
	}

	.student:nth-child(odd) {
		background-color: #eee;
	}

	.about-shadow {
		border-radius: 15rpx;
		box-shadow: 0rpx 0rpx 50rpx 0rpx rgba(0, 0, 0, 0.07);
		width: 650rpx;
		margin-left: 7%;
	}

	/* 页面 end*/
	.custom {
		background-color: #2C65F7;
	}

	.navcontent {
		height: 45px;
		display: -ms-flex;
		display: -webkit-flex;
		display: flex;
		justify-content: space-around;
		flex-direction: row;
		color: #FFFFFF;
	}

	.livePusher {
		width: 350px;
		height: 350px;
	}

	.livefater {
		display: -ms-flex;
		display: -webkit-flex;
		display: flex;
		justify-content: center;
		flex-direction: column;
		align-items: center;
		margin-top: 50rpx;
		margin-bottom: 50rpx;
		height: 350px;
	}

	.gaiimg {
		width: 350px;
		height: 350px;
		margin-top: -350px;
	}
</style>
