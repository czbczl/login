
<template>
  <div class="login_container">
    <!-- <img src="../assets/logo.png" alt=""> -->
    <div class="login_box">
      <div class="head_box">
        <img src="../assets/logo.png" alt />
      </div>
      <el-form
        :model="loginForm"
        ref="loginFormRef"
        :rules="loginFormRules"
        label-width="0px"
        class="lgform"
      >
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            v-model="loginForm.password"
            prefix-icon="iconfont icon-3702mima"
            type="password"
          ></el-input>
        </el-form-item>
        <el-form-item class="lgbt">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      loginForm: {
        username: "",
        password: ""
      },
      loginFormRules: {
        username: [
          { required: true, message: "请输入登录名称", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ],
        password: [
          { required: true, message: "请输入登录密码", trigger: "blur" },
          { min: 6, max: 15, message: "长度在6 到 15 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields();
    },
    // login() {
    //   this.$refs.loginFormRef.validate(async valid => {
    //     if (!valid) return;
    //     // const {data:res} = await this.$http.post("login", this.loginForm);
    //     this.$router.push("/home");
    //   });
    // }
    login() {
      var username = this.loginForm.username;
      var password = this.loginForm.password;
      var self = this;
      if (username == "" || password == "") {
        alert("帐号和密码不能为空");
      } else {
        //向服务器提交数据
        axios
          .post("http://localhost:5000/login", {
            username: username,
            password: password
          })
          .then(function(response) {
            //成功时服务器返回 response 数据
            // this.$router.push("/home");

            // alert(response.data);
            var st = response.data;
            var sts = "sts";
            if (st == sts) {
              self.$refs.loginFormRef.validate(async valid => {
                if (!valid) return;
                // const {data:res} = await this.$http.post("login", this.loginForm);
                self.$router.push("/home");
              });
            } else {
              alert("用户名或密码错误！");
            }
            // self.$refs.loginFormRef.validate(async valid => {
            //   if (!valid) {
            //     alert("用户名胡密码错误!");
            //   }
            //   // const {data:res} = await this.$http.post("login", this.loginForm);
            //   else {
            //     self.$router.push("/home");
            //   }
            // });
          })
          .catch(function(error) {
            console.log(error);
          });
      }
    }
  }
};
</script>
<style  scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
.head_box {
  height: 130px;
  width: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #ddd;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}
.head_box img {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}
.lgform {
  position: absolute;
  bottom: 0px;
  width: 100%;
  padding: 0 30px;
  box-sizing: border-box;
}
.lgbt {
  display: flex;
  justify-content: flex-end;
}
</style>
