<template>
  <div class="login_container">
    <div class="login_box">
      <div class="login_avatar">
        <img src="../assets/logo.png" alt="">
      </div>
        <el-form :model="loginForm" :rules="rules" ref="loginFormRef" label-width="0px" class="login_form">
          <el-form-item prop="username">
            <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input v-model="loginForm.password" type="password" prefix-icon="iconfont icon-3702mima"></el-input>
          </el-form-item>
           <el-form-item class="btn">
            <el-button type="primary" @click="login">登录</el-button>
            <el-button type="info" @click="res">重置</el-button>
          </el-form-item>
        </el-form>
      </div>
    </div>
</template>
<script>
export default {
  name: 'login',
  data () {
    return {
      rules: {
        username: [
          { required: true, message: '请输入登录账号', trigger: 'blur' },
          { min: 3, max: 8, message: '长度在 3 到 8 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' }
        ]
      },
      loginForm: {
        username: 'admin',
        password: '123456'
      }
    };
  },
  methods:{
    login() {
      this.$refs.loginFormRef.validate(async valid => {
          if(!valid) return this.$message.error('信息验证失败')
          const {data: res} = await this.$http.post('login', this.loginForm)
          if(res.meta.status != 200) return this.$message.error('登录失败') 
          this.$message.success('登录成功')
          window.sessionStorage.setItem('token', res.data.token);
          this.$router.push('/home')
        })
    },
    res() {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>
<style scoped lang="less">
  .login_container {
        background-color: #2b4b6b;
        height: 100%;
        .login_box {
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          width: 450px;
          height: 300px;
          background-color: #fff;
          border-radius: 3px;
          .login_avatar {
            width: 130px;
            height: 130px;
            border-radius: 50%;
            border: 1px solid #eee;
            padding: 10px;
            position: absolute;
            top: 0;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #fff;
            box-shadow: 0 0 10px #ddd;
            img {
              width: 100%;
              border-radius: 50%;
              background-color: #eee;
            }
          }
          .login_form {
                    position: absolute;
                    bottom: 0;
                    left:0;
                    width:100%;
                    padding:0 20px;
                    box-sizing: border-box;

                    .btn {
                        display: flex;
                        justify-content: flex-end;
                    }
                }
        }
  }
</style>
