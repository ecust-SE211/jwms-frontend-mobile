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
		      学生查看
		    </view>
		  </view>
		  
		  <view class="tn-flex tn-margin-left tn-margin-right tn-margin-top-sm">
			<view class="tn-padding-left-sm" style="width: 100%;">
			  <view class="tn-flex tn-flex-row-between tn-flex-col-between">
			    <view class="justify-content-item">
			      <text class="tn-color-cat tn-text-lg tn-text-bold">
				  考勤{{fid}}签到学生如下：</text>
			    </view>
			  </view>
			</view>
			</view>
		  </view>
		  
		  <view class="box">
		  		<view class="line"></view>
				<view class="about-shadow tn-margin-top-lg tn-padding-top-sm tn-padding-bottom-sm">
				  <view class="student" v-for="(item,index) in attend_students" :key="index">
				  	<tn-list-cell :hover="true" :unlined="true" :radius="true" :fontSize="30">
				  	  <view class="tn-flex tn-flex-col-center">
				  	    <text style="flex: 1;">{{item.name}}</text>
						    <text style="flex: 2;">已签到</text>
				  	  </view>
				  	</tn-list-cell>
				  </view>
          <view class="student" v-for="(item,index) in unattend_students" :key="index">
            <tn-list-cell :hover="true" :unlined="true" :radius="true" :fontSize="30">
              <view class="tn-flex tn-flex-col-center">
                <text style="flex: 1;">{{item.name}}</text>
                <text style="flex: 2;">未签到</text>
              </view>
            </tn-list-cell>
          </view>
				</view>
		  </view>
    <view>
      <video id="video" width="640" height="480" hidden="hidden"></video>
      <canvas id="canvas" style="position: absolute;left: 20%;right: 20%;width:480px;height: 320px"></canvas>
    </view>
    </view>
</template>

<script>
  import * as requestUtil from "@/utils/request";
  import axios from "axios";

  export default {
		data() {
			return {
        fid:'',
        class_id:'',
        students:[],// 学生姓名 从后端获取
        attend_students:[],
        unattend_students:[],
        faceApiLoaded: false
			};
		},
    onLoad(options) {
      this.fid = JSON.parse(decodeURIComponent(options.fid)).fid;
      this.class_id =JSON.parse(decodeURIComponent(options.fid)).clid;
      this.initStudent().then(e=> {
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
            faceapi.nets.ssdMobilenetv1.loadFromUri('../../static/models/ssd_mobilenetv1_model-weights_manifest.json'),
            faceapi.nets.faceLandmark68Net.loadFromUri('../../static/models/face_landmark_68_model-weights_manifest.json'),
            faceapi.nets.faceRecognitionNet.loadFromUri('../../static/models/face_recognition_model-weights_manifest.json'),
          ]);
          await this.startVideoStream();
        } catch (e) {
          console.error('Failed to load face-api.js models:', e);
        }
      };
      document.body.appendChild(faceapiminScript);
    },
    methods: {
      async startVideoStream() {
        if (!this.faceApiLoaded) return;
        const video = document.querySelector('video');
        const canvas = document.querySelector('canvas');
        const ctx = canvas.getContext('2d');
        canvas.width = video.width;
        canvas.height = video.height;
        navigator.mediaDevices.getUserMedia({ video: true, audio: false })
            .then(function(stream) {
              video.srcObject = stream;
              video.play();
              this.detectFacesInStream(video, canvas, ctx);
            }.bind(this))
            .catch(function(err) {
              console.log("An error occurred! " + err);
            });
      },
      detectFacesInStream(video, canvas, ctx) {
        setInterval( () => {
          ctx.drawImage(video, 0, 0, canvas.width, canvas.height);
          faceapi.detectSingleFace(canvas).withFaceLandmarks().withFaceDescriptor().then( result => {
            // 对检测到的人脸进行处理
            // 在人脸上画框
            ctx.strokeStyle = 'red';
            if(result) {
              const face = result;
              // const box = face.detection.box;
              // ctx.strokeRect(box.x, box.y, box.width, box.height);
              // ctx.stroke();
              // 人脸检验
              this.checkFace(face,canvas).then(async r => {
                if(r){
                }
              })
            }
          })
        }, 200); // 每隔一定时间检测一次
      },
      cameraShoot(video, startPoint, width, height) {
        const canvas = document.createElement("canvas");
        canvas.width = video.width;
        canvas.height = video.height;
        canvas.getContext("2d").drawImage(video, startPoint.x, startPoint.y, width, height,0,0, canvas.width, canvas.height)
        return canvas;
      },
      async checkFace(face,canvas) {
        const control = document.createElement("img")
        const url='../../static/faces/'
        for (let student of this.unattend_students){
          control.src = url + student.id +'.jpg';
          const controlface = await faceapi.detectSingleFace(control).withFaceLandmarks().withFaceDescriptor();
          if (face.descriptor) {
            const faceDistance = faceapi.euclideanDistance(controlface.descriptor, face.descriptor);
            // console.log(faceDistance);
            if(faceDistance < 0.4) {
              try{
                const box=face.detection.box;
                this.cameraShoot(canvas, box, box.width, box.height).toBlob((blob)=>{
                  // 将image转化成file文件
                  const formData = new FormData();
                  // 将文件添加到FormData对象中
                  formData.append('studentId', student.id);
                  formData.append('file', blob, 'image.png');
                  axios.post("http://localhost:8088/student/compare",formData,
                  {
                    headers: {
                      'Content-Type': 'multipart/form-data'
                    }
                  }).then(async res => {
                    if (res.data.code === '200') {
                      console.log(res.data.data);
                      if (res.data.data) {
                        try {
                          const res2 = await requestUtil.post("attendancerecord", {
                            id: '',
                            sid: student.id,
                            fid: this.fid,
                          });
                          console.log(res2.data);
                          if (res2.data.code === '200') {
                            alert(student.name + "签到成功");
                            window.location.reload();
                          }
                        } catch (e) {
                        }
                      }
                    }
                  });
                })
              }catch (e){}
              return true;
            }else {
              return false;
            }
          } else {
            return false;
          }
        }
      },
      async initAttendance(){
        const res = await requestUtil.get("attendancerecord/student",{fid:this.fid});
        if (res.data.code === '200') {
          const rec=res.data.data.records;
          for (let student of this.students) {
            this.find(rec, student.id).then(r=> {
              if(r){
                this.attend_students.push({
                  id: student.id,
                  name: student.name
                })
              } else{
                this.unattend_students.push({
                  id: student.id,
                  name: student.name
                })
              }
            })
          }
        }
      },
      async find(rec,id){
        for (let item of rec){
          if(item.sid === id) {
            return true;
          }
        }
        return false;
      },
      async initStudent() {
        const res = await requestUtil.get("classstudent/student",{clid:this.class_id});
        if (res.data.code === '200') {
          this.students = res.data.data;
        }
      },
      // 返回上一级
			back(){
				uni.navigateBack({
					delta: 1
				})
			},
		},

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

/* 页面 start*/
  .about-shadow {
    border-radius: 15rpx;
    box-shadow: 0rpx 0rpx 50rpx 0rpx rgba(0, 0, 0, 0.07);
	width: 650rpx;
	margin-left: 7%;
  }

  /* 页面 end*/
.custom{
  background-color: #2C65F7;
}
.navcontent{
  height: 45px;
  display: -ms-flex;
  display: -webkit-flex;
  display: flex;
  justify-content:space-around;
  flex-direction:row;
  color:#FFFFFF;
}
.livePusher{
  width: 350px;
  height: 350px;
}
.livefater{
  display: -ms-flex;
  display: -webkit-flex;
  display: flex;
  justify-content:center;
  flex-direction:column;
  align-items:center;
  margin-top: 50rpx;
  margin-bottom: 50rpx;
  height: 350px;
}
.gaiimg{
  width: 350px;
  height: 350px;
  margin-top: -350px;
}
</style>
