<template>
  <div class="login_container">
    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <!-- 表单区域 -->
      <el-form ref="loginFormRef" class="login_form" :rules="loginFormRules" :model="loginForm">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input prefix-icon="iconfont icon-user" v-model="loginForm.username" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <!-- 密码框 -->
        <el-form-item prop="password">
          <el-input type="password" prefix-icon="iconfont icon-3702mima" v-model="loginForm.password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <!-- 提交 -->
        <!-- 重置 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      //这是登录表单的数据绑定对象
      loginForm: {
        username: '',
        password: '',
      },
      //这是表单的验证规则对象
      loginFormRules: {
        //验证用户名是否合法
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' },
        ],
        //验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' },
        ],
      },
    }
  },
  methods: {
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    },
    login() {
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败，用户名或密码错误！')
        this.$message.success('登录成功！')
        //1.将登录成功之后的token，保存到客户端的sessionStorage中
        window.sessionStorage.setItem('token', res.data.token)
        //  1.1项目中除了登录之外的其他API接口，必须在登录之后才能访问
        //  1.2token只应在当前网站打开期间生效，所以将token值保存在sessionStorage中
        //2.通过编程式导航跳转到后台主页，路由地址是 /home
        this.$router.replace('/home')
      })
    },
  },
}
</script>

<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);

    .avatar_box {
      width: 130px;
      height: 130px;
      border-radius: 50%;
      border: 1px solid #eee;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;

      padding: 10px;
      img {
        width: 100%;
        height: 100%;
        background-color: #eee;
        border-radius: 50%;
      }
    }
  }
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
.btns {
  // float: right;
  display: flex;
  justify-content: flex-end;
}
</style>
