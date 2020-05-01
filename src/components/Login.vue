<template>
    <div id="app">
        <!-- 登录框 -->
        <div class="login">
            <!-- 头像部分 -->
            <div class="toux">
                <img src="../assets/logo.png" alt="">
            </div>
            <!-- 登录表单 -->
            <div class="login_form" >
                <!-- 用户名框 -->
                <el-form  :model="form" :rules="rules" ref="form_ref" >
                    <el-form-item prop="username">
                        <el-input prefix-icon="el-icon-user-solid" v-model="form.username"></el-input>
                    </el-form-item>
                <!-- 密码框 -->
                    <el-form-item prop="password" >
                        <el-input prefix-icon="el-icon-lock" v-model="form.password" type="password"></el-input>
                    </el-form-item>
                </el-form>
                <el-row class="bth">
                    <!-- 登录按钮 -->
                    <el-button type="primary" @click="load">登录</el-button>
                    <!-- 重置按钮 -->
                    <el-button type="info" @click="reset">重置</el-button>
                </el-row>
            </div>
        </div>
    </div>
</template>
<script>
export default {
  data () {
    return {
      // 表单数据绑定对象
      form: {
        username: '',
        password: ''
      },
      // 表单检验对象
      rules: {
        username: [
          { required: true, message: '请输入用户名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入用户密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 重置数据
    reset () {
      this.$refs.form_ref.resetFields()
    },
    load () {
      var that = this
      this.$refs.form_ref.validate(valid => {
        if (valid) {
          this.$http.post('login', this.form).then(function (res) {
            if (res.data.meta.status !== 200) {
              that.$message.error('登陆失败')
            } else {
              that.$message.success('登陆成功')
              window.sessionStorage.setItem('token', res.data.data.token)
              that.$router.push('/home')
            }
          })
        }
      })
    }
  }
}
</script>
<style lang="less" scoped>
#app{
    width: 100%;
    background-color: rgb(14, 20, 25);
}
.login {
    width: 400px;
    height: 300px;
    position: relative;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    background-color: #fff;
    border-radius: 10px;
}
.toux,img{
    width: 100px;
    height: 100px;
    border: 1px solid black;
    border-radius: 50%;
    background-color: #fff;
}
.toux{
    padding: 6px;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
}
.login_form{
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 15px;
    box-sizing: border-box;
}
.bth{
    display: flex;
    justify-content: flex-end;
}
</style>
