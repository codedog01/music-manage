<template>
  <div class="login-wrap">
    <div class="ms-title" style="color: white">Xuan-music 后台管理</div>
    <div class="ms-login">
      <el-form
        ref="ruleForm"
        class="demo-ruleForm myForm"
        :model="ruleForm"
        :rules="rules"
      >
          <input  prop="username" id="username" style=" background: rgba(45,45,45,.15)"  class="myInput" v-model="ruleForm.username" placeholder="username"></input>
          <input
            prop="password"
            class="myInput"
            type="password"
            placeholder="password"
            v-model="ruleForm.password"
            @keyup.enter.native="submitForm('ruleForm')"
          ></input>
        <el-button class="myBtn" type="primary" @click="submitForm">Login</el-button>

<!--        <p style="font-size:12px;line-height:30px;color:#999;">Tips : 用户名和密码要写数据库里的。</p>-->
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

<style>

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
    color: white;
  }

  .ms-login {
    position: absolute;
    text-align: center;
    left: 50%;
    top: 50%;
    width: 300px;
    height: 160px;
    margin: -150px 0 0 -190px;
    padding: 40px;
    border-radius: 5px;
  }

  .myInput{
    color: rgba(255,255,255,.75);
    padding: 0;
    width: 90%;
    margin-top: 15px;
    background: rgba(45,45,45,.15);
    -moz-border-radius: 6px;
    -webkit-border-radius: 6px;
    border-radius: 6px;
    border: 1px solid rgba(255,255,255,.15);
    -moz-box-shadow: 0 2px 3px 0 rgba(0,0,0,.1) inset;
    -webkit-box-shadow: 0 2px 3px 0 rgba(0,0,0,.1) inset;
    box-shadow: 0 2px 3px 0 rgba(0,0,0,.1) inset;
    font-family: 'PT Sans', Helvetica, Arial, sans-serif;
    font-size: 14px;
    text-shadow: 0 1px 2px rgba(0,0,0,.1);
    transition: all .2s;
    text-align: center;
    height: 50px;
  }

.myInput:focus{
  color: turquoise;
  outline: none;
  width: 100%;
  -moz-box-shadow:
    0 2px 3px 0 rgba(0,0,0,.1) inset,
    0 2px 7px 0 rgba(0,0,0,.2);
  -webkit-box-shadow:
    0 2px 3px 0 rgba(0,0,0,.1) inset,
    0 2px 7px 0 rgba(0,0,0,.2);
  box-shadow:
    0 2px 3px 0 rgba(0,0,0,.1) inset,
    0 2px 7px 0 rgba(0,0,0,.2);
}

  .myInput{

}

  .myBtn:hover{
    background-color: rgba(255,255,255,.75);
    color: rgb(18,181,130);
  }

  .myBtn{
    background: rgba(255,255,255,.55);
    cursor: pointer;
    width: 90%;
    height: 44px;
    margin-top: 25px;
    padding: 0;
    border: 0;
    -moz-border-radius: 6px;
    -webkit-border-radius: 6px;
    border-radius: 6px;
    -moz-box-shadow:
      0 15px 30px 0 rgba(255,255,255,.25) inset,
      0 2px 7px 0 rgba(0,0,0,.2);
    -webkit-box-shadow:
      0 15px 30px 0 rgba(255,255,255,.25) inset,
      0 2px 7px 0 rgba(0,0,0,.2);
    box-shadow:
      0 15px 30px 0 rgba(255,255,255,.25) inset,
      0 2px 7px 0 rgba(0,0,0,.2);
    font-family: 'PT Sans', Helvetica, Arial, sans-serif;
    font-size: 14px;
    font-weight: 700;
    color: rgb(18,181,130);
    text-shadow: 0 1px 2px rgba(0,0,0,.1);
    transition: all .2s;
  }
</style>
