<template>
  <div class="login-wrap">
    <div class="ms-title">Xuan-music 后台管理</div>
    <div class="ms-login">
      <el-form
        ref="ruleForm"
        class="demo-ruleForm myForm"
        :model="ruleForm"
        :rules="rules"
      >
        <el-form-item prop="username" id="username">
          <el-input class="myInput" v-model="ruleForm.username" placeholder="username"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            class="myInput"
            type="password"
            placeholder="password"
            v-model="ruleForm.password"
            @keyup.enter.native="submitForm('ruleForm')"
          ></el-input>
        </el-form-item>
        <div class="login-btn">
          <el-button type="primary" @click="submitForm">Login</el-button>
        </div>
        <p style="font-size:12px;line-height:30px;color:#999;">Tips : 用户名和密码要写数据库里的。</p>
      </el-form>
    </div>
  </div>
</template>

<script>
import {mixin} from '../mixins'
import { HttpManager } from '../api/index'
export default {
  mixins: [mixin],
  data: function () {
    return {
      ruleForm: {
        username: 'admin',
        password: '123'
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' }
        ],
        password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
      }
    }
  },
  methods: {
    submitForm () {
      let params = new URLSearchParams()
      params.append('name', this.ruleForm.username)
      params.append('password', this.ruleForm.password)
      HttpManager.getLoginStatus(params)
        .then(res => {
          if (res.code === 1) {
            this.$router.push('/Info')
            this.notify('欢迎回来', 'success')
          } else {
            this.notify('登录失败', 'error')
          }
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style scoped>
  #username:focus{
    width: 110%;
  }
.login-wrap {
  animation: 12s bgChange infinite ;
}

@keyframes bgChange {
  0%{
    position: relative;
    background: url('../assets/img/1.jpg') fixed center;
    background-size: cover;
    width: 100%;
    height: 100%;
  }
  33%{
    position: relative;
    background: url('../assets/img/2.jpg') fixed center;
    background-size: cover;
    width: 100%;
    height: 100%;

  }
  66%{
    position: relative;
    background: url('../assets/img/3.jpg') fixed center;
    background-size: cover;
    width: 100%;
    height: 100%;
  }
  100%{
    position: relative;
    background: url('../assets/img/1.jpg') fixed center;
    background-size: cover;
    width: 100%;
    height: 100%;
  }
}

.ms-title {
  position: absolute;
  top: 50%;
  width: 100%;
  margin-top: -230px;
  text-align: center;
  font-size: 30px;
  font-weight: 600;
  color: #fff;
}

.ms-login {
  position: absolute;
  left: 50%;
  top: 50%;
  width: 300px;
  height: 160px;
  margin: -150px 0 0 -190px;
  padding: 40px;
  border-radius: 5px;
  background: #fff;
}

.login-btn {
  text-align: center;
}

.login-btn button {
  width: 100%;
  height: 36px;
}
</style>
