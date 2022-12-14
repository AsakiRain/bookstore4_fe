<template>
  <div id="card-wrapper">
    <div id="card">
      <div id="card-content">
        <div id="card-left">
          <div id="register-title">注册</div>
          <a-form
            id="register-form"
            :model="registerForm"
            @submit="handleRegister"
          >
            <a-form-item
              field="username"
              label="用户名"
              hide-label
              :validate-trigger="['blur']"
              :rules="[
                { required: true, message: '用户名不能为空' },
                {
                  match: /^[a-zA-Z0-9_]{2,20}$/,
                  message: '只能输入2-20位字母、数字和下划线',
                },
              ]"
            >
              <a-input
                v-model="registerForm.username"
                placeholder="请输入用户名"
              >
                <template #prefix>
                  <icon-user/>
                </template>
              </a-input>
            </a-form-item>
            <a-form-item
              field="nickname"
              label="昵称"
              hide-label
              :validate-trigger="['blur']"
              :rules="[
                { required: true, message: '昵称不能为空' },
                {
                  match: /^[\u4E00-\u9FA5A-Za-z0-9_]{2,20}$/,
                  message: '只能输入2-20位可见字符',
                },
              ]"
            >
              <a-input v-model="registerForm.nickname" placeholder="请输入昵称">
                <template #prefix>
                  <icon-at/>
                </template>
              </a-input>
            </a-form-item>
            <a-form-item
              field="password"
              label="密码"
              hide-label
              :validate-trigger="['blur']"
              :rules="[
                { required: true, message: '密码不能为空' },
                {
                  match: /[\x21-\x7e]{8,36}$/,
                  message: '只能输入8-36位字母、数字和符号',
                },
              ]"
            >
              <a-input
                v-model="registerForm.password"
                placeholder="请输入密码"
                type="password"
                allow-clear
              >
                <template #prefix>
                  <icon-lock/>
                </template>
              </a-input>
            </a-form-item>
            <a-form-item field="sex" label="性别"
                         :validate-trigger="['blur']"
                         :rules="[
                { required: true, message: '请选择性别' },
              ]">
              <a-select placeholder="请选择性别" v-model="registerForm.sex" allow-create>
                <a-option>未指定</a-option>
                <a-option>男</a-option>
                <a-option>女</a-option>
              </a-select>
            </a-form-item>
            <div id="button-wrapper">
              <a-button type="primary" long html-type="submit">注册</a-button>
              <a-button type="outline" long @click="$router.push('/login')">登录</a-button>
            </div>
          </a-form>
        </div>
        <div id="card-right"></div>
      </div>
    </div>
  </div>
</template>
<script lang="ts" setup>
  import { reactive } from 'vue';
  import { IconUser, IconLock, IconAt } from '@arco-design/web-vue/es/icon';
  import { useRouter } from 'vue-router';
  import { Message, ValidatedError } from '@arco-design/web-vue';
  import type { RegisterForm } from '@/models/auth';
  import useUserStore from '@/store/user';
  import sleep from '@/utils/sleep';

  const router = useRouter();
  const userStore = useUserStore();
  const registerForm = reactive<RegisterForm>({
    username: 'rainchen',
    nickname: 'rainchen',
    password: 'ssr129631',
    sex: '男'
  });

  const handleRegister = async ({
                                  values,
                                  errors
                                }: { values: RegisterForm, errors: Record<string, ValidatedError> }) => {
    if (errors) return;
    console.log(values);
    if (await userStore.register(values)) {
      Message.success('注册成功');
      await sleep(500);
      await router.push('/');
    }
  };
</script>
<style lang="css">
  #card-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  #card {
    background-color: #ffffff;
    border-radius: 12px;
  }

  #card-content {
    display: flex;
  }

  #card-left,
  #card-right {
    height: 500px;
  }

  #card-left {
    width: 360px;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 36px;
    justify-content: center;
  }

  #card-right {
    width: 340px;
    background-image: url("/img/card-right.jpeg");
    background-size: cover;
    background-repeat: no-repeat;
    border-radius: 0 12px 12px 0;
  }

  #register-title {
    text-align: center;
    font-size: 36px;
    margin-bottom: 36px;
  }

  #register-form {
    width: 100%;
  }

  #button-wrapper {
    margin-top: 12px;
    display: flex;
    flex-direction: column;
    row-gap: 12px;
  }
</style>
