<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/img/logo.png" alt id="logo" />
      </div>
      <el-form ref="loginFormref" id="login_form" :model="loginForm" :rules="loginRules">
        <el-form-item prop="username">
          <el-input prefix-icon="el-icon-user-solid" v-model="loginForm.username"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input
            placeholder="请输入密码"
            prefix-icon="el-icon-lock"
            show-password
            v-model="loginForm.password"
          ></el-input>
        </el-form-item>
        <el-form-item id="btns">
          <el-button type="primary" @click="loginCheck()">登录</el-button>
          <el-button type="info" @click="resetloginFrom()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      loginRules: {
        username: [
          { required: true, message: '请输入账号', trigger: 'blur' },
          { min: 3, max: 12, message: '长度在3-12之间', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 16, message: '长度在6-16之间', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    resetloginFrom () {
      console.log(this)
      this.$refs.loginFormref.resetFields()
    },
    loginCheck () {
      this.$refs.loginFormref.validate(async flag => {
        if (!flag) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) { return this.$message.error('登陆失败') }
        this.$message.success('登陆成功')
        console.log(res)
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<style Lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}
.login_box {
  background-color: #fff;
  height: 300px;
  width: 450px;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -225px;
  margin-top: -150px;
}
.avatar_box {
  width: 130px;
  height: 130px;
  border: 1px solid #eee;
  border-radius: 50%;
  box-shadow: 0 0 10px #ddd;
  padding: 10px;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
}
#logo {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background-color: #eee;
}
#login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 30px;
  box-sizing: border-box;
}
#btns {
  display: flex;
  justify-content: flex-end;
}
</style>
