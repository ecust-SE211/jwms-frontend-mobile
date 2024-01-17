<template>
  <div class="container">
    <div class="login-wrapper">
      <div class="header">教师登录</div>
      <div class="form-wrapper">
        <form method="post">
          <div class="login">
            <input type="text" id="userid" placeholder="请输入账号" class="input-item" />
            <input type="password" id="password" placeholder="请输入密码" class="input-item" />
            <button @click="login()">登录</button>
          </div>
        </form>
      </div>
    </div>
  </div>

</template>

<script>
  // 登录请求
  import requestUtil from "../../utils/request";
  export default {
    methods: {
      login() {
        const userid=document.getElementById("userid").querySelector("input").value;
        const password=document.getElementById("password").querySelector("input").value;
        if (!userid || !password) {
          alert('请输入账号和密码!')
          return
        }
        const res = requestUtil.post("teacher/login", {
          id: userid,
          name: "",
          password: password
        })
        res.then(r => {
          console.log(r.data.code)
          if (r.data.code === '200') {
            console.log('登录成功');
            localStorage.setItem("teacher", r.data.data.id)
            uni.navigateTo({
              url: "/pages/home/home"
            })
          }else {
			alert('登录失败')
            console.log('登录失败')
          }
        });
      }
    },
  }
</script>

<style scoped lang="scss">
//form居中
* {
  padding: 0;
  margin: 0;
  font-family: 'Open Sans Light';
  letter-spacing: .05em;
}
html {
  height: 100%;
}

body {
  height: 100%;
}

.container {
  height: 100%;
  background-image: linear-gradient(to right,#fbc2eb,#a6c1ee);
}

.login-wrapper {
  background-color: #fff;
  width: 250px;
  height: 500px;
  padding: 0 50px;
  position: absolute;
  left: 50%;
  border-radius: 15px;
  top:50%;
  transform: translate(-50%,-50%);
}
.login-wrapper .header {
  font-size:  30px;
  font-weight: bold;
  text-align: center;
  line-height: 200px;
}

.login-wrapper .form-wrapper .input-item {
  width: 100%;
  margin-bottom: 20px;
  border-bottom: 2px solid rgb(128,125,125);
  font-size: 15px
}

</style>