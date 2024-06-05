<template>
  <div class="login-container">
    <el-row class="main-row" style="height: 100%">
      <el-col>
        <el-form
          class="login-form"
          :model="loginForm"
          :rules="rules"
          ref="loginFormRules"
        >
          <h1>Hello</h1>
          <h2>欢迎来到BlueBuy</h2>
          <el-form-item prop="username">
            <el-input
              v-model="loginForm.username"
              :prefix-icon="User"
              placeholder="admin"
            ></el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input
              v-model="loginForm.password"
              type="password"
              :prefix-icon="Lock"
              show-password
              placeholder="111111"
            ></el-input>
          </el-form-item>
          <el-form-item>
            <el-button
              class="login-btn"
              type="primary"
              size="default"
              @click="login"
              :loading="loading"
            >
              登录
            </el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>

<script setup lang="ts" name="LoginIndex">
import { reactive, ref } from 'vue'
import { User, Lock } from '@element-plus/icons-vue'
// 引入用户相关的小仓库
import useUserStore from '@/store/modules/user'
import { useRouter, useRoute } from 'vue-router'
import { ElNotification } from 'element-plus'
import { getTime } from '@/utils/time'

// 获取路由器
let $router = useRouter()
let useStore = useUserStore()
// 获取路由对象
let $route = useRoute()
// 定义变量，控制按钮加载效果
let loading = ref(false)
// 收集账号密码
let loginForm = reactive({
  username: '',
  password: '',
})
// 定义表单规则
let loginFormRules = ref()

// 登录按钮的回调
const login = async () => {
  await loginFormRules.value.validate()
  loading.value = true
  // 通知仓库发送登录请求
  // 请求成功，首页展示数据
  // 请求失败，弹出登录失败
  try {
    await useStore.userLogin(loginForm)
    // 编程式导航跳转到数据首页
    // 判断有没有query参数，如果有就往query跳转，没有就跳转到首页
    // eslint-disable-next-line @typescript-eslint/no-explicit-any
    let redirect: any = $route.query.redirect
    $router.push({ path: redirect || '/' })
    // 登陆成功的提示
    ElNotification({
      type: 'success',
      message: '登陆成功',
      title: `Hi, ${getTime()}好`,
    })
  } catch (error) {
    // 登陆失败的提示
    ElNotification({
      type: 'error',
      message: (error as Error).message,
    })
  } finally {
    // 登陆后，加载效果消失
    loading.value = false
  }
}

// 自定义校验规则函数
// const validatorUserName = (
//   rule: object,
//   value: string,
//   callback: (error?: Error) => void,
// ) => {
//   if (value.length >= 2) {
//     callback()
//   } else {
//     callback(new Error('账号长度至少为2位'))
//   }
// }
// const validatorPassword = (
//   rule: object,
//   value: string,
//   callback: (error?: Error) => void,
// ) => {
//   if (value.length === 0) {
//     callback(new Error('密码不能为空！'))
//   } else {
//     callback()
//   }
// }

// 定义表单校验需要的配置对象
const rules = {
  username: [
    {
      required: true,
      min: 3,
      max: 10,
      message: '账号长度至少为3位',
      trigger: 'change',
    },
    // 自定义校验规则
    // {
    //   trigger: 'change',
    //   validator: validatorUserName,
    // },
  ],
  password: [
    {
      required: true,
      message: '密码不能为空',
      trigger: 'blur',
    },
    // {
    //   trigger: 'change',
    //   validator: validatorPassword,
    // },
  ],
}
</script>

<style scoped lang="scss">
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100vh;
  background-image: url('@/assets/images/background.jpg');
  background-repeat: no-repeat;
  background-size: cover;

  .main-row {
    justify-content: center;
    align-items: center;
    width: 50%;
    max-width: 500px;
  }

  .login-form {
    padding: 20px;
    background-image: url('@/assets/images/login_form.png');
    background-size: cover;
    background-repeat: no-repeat;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgb(0 0 0 / 10%);

    h1 {
      color: white;
      font-size: 40px;
    }

    h2 {
      font-size: 20px;
      color: white;
      margin: 20px 0;
    }

    .login-btn {
      width: 100%;
    }
  }
}
</style>
