<template>
  <div class="login-container">
    {{ loginUrl }}

    <form :action="loginUrl" method="post">
      <input type="text" name="phone" :value="loginForm.phone" />
      <input type="text" name="password" :value="loginForm.password" />
      <input type="submit" value="提交" />
    </form>
    <div class="login-form">
      <a-input v-model:value="loginUrl" />

      <a-form
        ref="loginFormRef"
        :model="loginForm"
        name="normal_login"
        :rules="loginRules"
        @finishFailed="onFinishFailed"
      >
        <a-form-item
          label="phone"
          name="phone"
          :rules="[{ required: true, message: 'Please input your username!' }]"
        >
          <a-input v-model:value="loginForm.phone" :autoComplete="false">
            <template #prefix>
              <UserOutlined class="site-form-item-icon" />
            </template>
          </a-input>
        </a-form-item>

        <a-form-item
          label="Password"
          name="password"
          :rules="[{ required: true, message: 'Please input your password!' }]"
        >
          <a-input-password v-model:value="loginForm.password" :autoComplete="false">
            <template #prefix>
              <LockOutlined class="site-form-item-icon" />
            </template>
          </a-input-password>
        </a-form-item>

        <!-- <div class="login-form-wrap">
          <a-form-item name="remember" no-style>
            <a-checkbox v-model:checked="loginForm.remember">Remember me</a-checkbox>
          </a-form-item>
          <a class="login-form-forgot" href="">Forgot password</a>
        </div> -->

        <a-form-item>
          <a-button type="primary" @click="onFinish" class="login-form-button"> Log in </a-button>
          Or
          <a href="">register now!</a>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>

<script lang="ts" setup>
import type { FormInstance } from "ant-design-vue/es/form/Form"
import type { Rule } from "ant-design-vue/es/form/interface"
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue"
import http from "@/utils/http"
import { ref, reactive } from "vue"

const loginFormRef = ref<FormInstance>()
const loginUrl = ref("http://auth.duobanginfo.com:7878/auth-server/auth/login?clientId=blast")
const activeKey = ref("user")
const loginForm = reactive({
  username: "15315115821",
  password: "123456",
  phone: "15315115821",
  code: ""
})

const state = reactive({
  loading: false,
  phoneCode: "获取验证码",
  codeStatus: false,
  currentCode: null
})

const loginRules: Record<string, Rule[]> = {
  username: [{ required: true, trigger: "blur" }],
  password: [{ required: true, trigger: "blur" }],
  phone: [{ required: true, trigger: "blur" }],
  code: [{ required: true, trigger: "blur" }]
}

const onFinish = async () => {
  const res = await loginFormRef.value?.validateFields()
  http.defaults.headers.common["X-Requested-With"] = "XMLHttpRequest"
  const body = await http.post(loginUrl.value, res)
  console.log(body)

  console.log("success")
  console.log(res)
}

const onFinishFailed = () => {
  console.log("success")
}

const getCode = () => {
  console.log("success")
}
</script>

<style lang="less" scoped>
.login-container {
  background: aliceblue;
  // background-image: url("@/assets/login/background.jpeg");
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100vh;
}
.login-form {
  width: 400px;
}
</style>
<!-- <style lang="less">
.login-container {
  .ant-tabs-bar {
    border-bottom: none !important;
  }
  .ant-input-affix-wrapper .ant-input:not(:first-child) {
    padding-left: 35px !important;
  }
  .ant-tabs-bar {
    margin-bottom: 30px !important;
  }
}
</style> -->
