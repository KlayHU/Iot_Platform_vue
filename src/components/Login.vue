<!--每个vue单文件组件由三部分组成，分别是结构、样式、行为-->
<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <!-- 头像区域 -->
                <img src="../assets/logo.png" alt="">
            </div>
            <!-- 登录表单区域 -->
            <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" class="login_form" label-width="0px">
                <!-- 用户名 -->
                <el-form-item prop="username">
                    <el-input v-model="loginForm.username" prefix-icon="el-icon-user-solid"></el-input>
                </el-form-item>
                <!-- 密码 -->
                <el-form-item prop="password">
                    <el-input v-model="loginForm.password" prefix-icon="el-icon-paperclip"></el-input>
                </el-form-item>
                <!-- 按钮区域 -->
                <el-form-item class="btns">
                    <el-button type="primary" @click="login">登录</el-button>
                    <el-button type="info" @click="reset">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>
<!-- 行为区域 -->
<script>
import Qs from 'qs'
export default {
  data () {
    return {
      // 登录表单数据绑定对象
      loginForm: {
        username: 'admin1',
        password: '123123',
        type: 'admin'
      },
      // 表单验证规则对象
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入昵称', trigger: 'blur' },
          { min: 4, max: 10, message: '长度在 4 到 10 个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 点击重置按钮，重置登录表单
    reset () {
      // console.log(this)
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      this.$refs.loginFormRef.validate(async valid => {
      // console.log(valid)
        if (!valid) return
        var data = Qs.stringify({ username: this.loginForm.username, password: this.loginForm.password, type: this.loginForm.type })
        const result = await this.$http.post('/account/login', data, { headers: { 'Content-Type': 'application/x-www-form-urlencoded' } })
        // const result = await this.$http.get('/account/login', this.loginForm)
        console.log(result)
      })
    }
  }
}
</script>
<!--加上scoped表示样式只在当前组件内生效，单文件组件建议使用scoped标签-->
<style lang="less" scoped>
.login_container{
    background-color: rgba(44, 131, 218, 0.333);
    height: 100%;
}

.login_box{
    width: 460px;
    height: 365px;
    background-color: #ffffff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .avatar_box{
        height: 140px;
        width: 140px;
        border: 1px solid #eeeeee;
        border-radius: 50%;
        padding: 10px;
        box-shadow: 0 0 10px #dddddd;
        position: absolute;
        left: 50%;
        transform: translate(-50%,-50%);
        background-color: #ffffff   ;
        img{
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background-color: #eee;
        }
    }
}

.login_form{
    position: absolute;
    bottom: 0%;
    width: 100%;
    padding: 0 40px;
    box-sizing: border-box;
}

.btns{
    display: flex;
    justify-content: flex-end;
}
</style>
