<template>
  <div class="wrap">
    <div class="img-box">
      <img src="../assets/img/logo_transparent.png" alt="myId"/>
      <div class="back" @click="handleBack">
        <el-icon><ArrowLeft /></el-icon>
        <span>返回</span>
      </div>

    </div>
    <div class="login-box">
      <p>使用12位助记词登陆现有身份</p>
        <div class="demo-input-size">
          <span>1.</span>
          <el-input
              v-model="input1"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
          <span>2.</span>
          <el-input v-model="input2" class="w-50 m-2" placeholder="Please Input" show-password />
          <span>3.</span>
          <el-input
              v-model="input3"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
        </div>
        <div class="demo-input-size">
          <span>4.</span>
          <el-input
              v-model="input1"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
          <span>5.</span>
          <el-input
              v-model="input2"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
          <span>6.</span>
          <el-input
              v-model="input3"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
        </div>
        <div class="demo-input-size">
          <span>7.</span>
          <el-input
              v-model="input1"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
          <span>8.</span>
          <el-input
              v-model="input2"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
          <span>9.</span>
          <el-input
              v-model="input3"
              class="w-50 m-2"
              placeholder="Please Input"
              show-password
          />
        </div>
      <div class="demo-input-size-last">
        <span>10.</span>
        <el-input
            v-model="input1"
            class="w-50 m-2"
            placeholder="Please Input"
            show-password
        />
        <span>11.</span>
        <el-input
            v-model="input2"
            class="w-50 m-2"
            placeholder="Please Input"
            show-password
        />
        <span>12.</span>
        <el-input
            v-model="input3"
            class="w-50 m-2"
            placeholder="Please Input"
            show-password
        />
      </div>
      <div class="submit">
        <el-checkbox v-model="checked" label="同意" />
        <el-link href="https://element-plus.org" type="primary" target="_blank">使用条款</el-link>
      </div>
      <el-button class="login-button" type="primary" :disabled="!checked" @click="handleLogin">登陆</el-button>
    </div>
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
const checked = ref(false)
const input1 = ref()
const input2 = ref('')
const input3 = ref('')
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
const handleBack = () =>{
  router.push('/login')
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
.wrap {
  position: relative;
  width: 100%;
  height: 100%;
  background-image: url(../assets/img/login.svg);
  background-size: 100%;
  .img-box {
    text-align: left;
    img {
      position: relative;
      vertical-align: middle;
      left: 620px;
      top: 60px;
      height: 160px;
      width: 160px;
    }
    .back{
      position: relative;
      left: 30px;
      top: -120px;
    }
    .back:hover{
      color: #4782ad;
      position: relative;
      left: 26px;
    }
  }
  .login-box{
    position: absolute;
    top: 200px;
    left: 35%;
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
      font-size: 20px;
      font-weight: bold;
      margin-top: 50px;
      margin-bottom: 30px;
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
}
</style>
