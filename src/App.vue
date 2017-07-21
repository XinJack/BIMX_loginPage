<template>
  <div id="app">
    <el-row>
      <el-col>
        <el-form label-position="right" label-width="80px" :model="loginForm" :rules="rules" ref="loginForm">
          {% csrf_token %}
          <el-form-item label="用户名" prop="username">
            <el-input v-model="loginForm.username"></el-input>
          </el-form-item>
          <el-form-item label="密码" prop="password">
            <el-input type="password" v-model="loginForm.password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="login">登录</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data(){
    return {
      loginForm: {
        username: '',
        password: ''
      },
      rules: {
        username: [{required: true, message: '用户名不能为空'}],
        password: [{required: true, message: '密码不能为空'}]
      }
    }
  },
  methods: {
    login(){
      this.$refs['loginForm'].validate((valid) => {
        if(valid){
          axios.post('/login', {
            username: this.loginForm.username,
            password: this.loginForm.password
          }).then((response) => {
            response = response.data;
            if(response.code === 'success'){
              window.location.href = '/';
            }else{
              this.$alert(response.message, '登录失败', {
                  confirmButtonText: '我知道了'
              });
            }
          }).catch((err) => {
            console.log(err);
            this.$alert('服务器错误，请稍后重试', '登录失败', {
              confirmButtonText: '我知道了'
            });
          });
        }
      });
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
#app
  width: 50%
  margin: 100px auto
  .el-alert
    margin-bottom: 22px
    width: 100%
</style>
