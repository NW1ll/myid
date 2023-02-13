<template>
  <div class="p">
    <p>助记词</p>
  </div>
  <div class="login-box">
    <p>请重新输入您的助记词</p>
    <div class="demo-input-size">
      <span>1.</span>
      <el-input
          v-model="input1"
          class="w-50 m-2"
      />
      <span>2.</span>
      <el-input v-model="input2" class="w-50 m-2" />
      <span>3.</span>
      <el-input
          v-model="input3"
          class="w-50 m-2"

      />
    </div>
    <div class="demo-input-size">
      <span>4.</span>
      <el-input
          v-model="input1"
          class="w-50 m-2"

      />
      <span>5.</span>
      <el-input
          v-model="input2"
          class="w-50 m-2"

      />
      <span>6.</span>
      <el-input
          v-model="input3"
          class="w-50 m-2"

      />
    </div>
    <div class="demo-input-size">
      <span>7.</span>
      <el-input
          v-model="input1"
          class="w-50 m-2"

      />
      <span>8.</span>
      <el-input
          v-model="input2"
          class="w-50 m-2"

      />
      <span>9.</span>
      <el-input
          v-model="input3"
          class="w-50 m-2"

      />
    </div>
    <div class="demo-input-size-last">
      <span>10.</span>
      <el-input
          v-model="input1"
          class="w-50 m-2"

      />
      <span>11.</span>
      <el-input
          v-model="input2"
          class="w-50 m-2"

      />
      <span>12.</span>
      <el-input
          v-model="input3"
          class="w-50 m-2"

      />
    </div>
    <el-button class="login-button" type="primary" @click="handleLogin">注册并登陆</el-button>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from 'vue';
import { useTagsStore } from '../store/tags';
import { usePermissStore } from '../store/permiss';
import { useRouter } from 'vue-router';
import { ElMessage } from 'element-plus';
import type { FormInstance, FormRules } from 'element-plus';

interface LoginInfo {
  username: string;
  password: string;
}
const show = ref(true)
const router = useRouter();
const param = reactive<LoginInfo>({
  username: 'admin',
  password: '123123'
});

const rules: FormRules = {
  username: [
    {
      required: true,
      message: '请输入用户名',
      trigger: 'blur'
    }
  ],
  password: [{ required: true, message: '请输入密码', trigger: 'blur' }]
};
const permiss = usePermissStore();
const login = ref<FormInstance>();
const handleBack = () => {
  router.go(-1)
  console.log(router)
  if(router.options.history.state.back === '/register'){
    show.value = true
  }
}
const handleLogin = () => {
  router.push('/dashboard')
}
const submitForm = (formEl: FormInstance | undefined) => {
  if (!formEl) return;
  formEl.validate((valid: boolean) => {
    if (valid) {
      ElMessage.success('登录成功');
      localStorage.setItem('ms_username', param.username);
      const keys = permiss.defaultList[param.username == 'admin' ? 'admin' : 'user'];
      permiss.handleSet(keys);
      localStorage.setItem('ms_keys', JSON.stringify(keys));
      router.push('/');
    } else {
      ElMessage.error('登录成功');
      return false;
    }
  });
};

const tags = useTagsStore();
tags.clearTags();
</script>

<style lang="less" scoped>
.login-box{
  position: absolute;
  top: 250px;
  left: 36%;
  width: 600px;
  border: 1px #a0a0a6 solid;
  box-sizing: border-box;
  background-color: var(--bg-color, #fff);
  border-radius: 20px;
  text-align: center;
  .submit{
    margin-top: 20px;
    a{
      font-size: 15px;
      position: relative;
      top: -2px;
    }
  }
  .login-button{
    margin: 10px 0 30px 0;
  }
  p{
    font-size: 16px;
    margin-top: 20px;
    margin-bottom: 30px;
    color: red;
  }
  .demo-input-size-last{
    margin: 0 45px;
    display: flex;
    justify-content: space-between;
    span{
      line-height: 52px;
      font-size: 16px;
      margin: 0 2px;
    }
    .w-50{
      width: 10rem;
    }
    .m-2{
      margin:0.5rem;
    }
  }
  .demo-input-size{
    margin: 0 50px;
    display: flex;
    justify-content: space-between;
    span{
      line-height: 52px;
      font-size: 16px;
      margin: 0 5px;
    }
    .w-50{
      width: 12.5rem;
    }
    .m-2{
      margin:0.75rem 0.5rem;
    }
  }
}
.p {
  position: relative;
  left: -26px;
  text-align: center;
  font-size: 32px;
  font-weight: bold;
}
</style>