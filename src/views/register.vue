<template>
  <div class="wrap">
    <div class="img-box">
      <img src="../assets/img/logo_transparent.png" alt="myId"/>
      <div class="back" @click="handleBack">
        <el-icon><ArrowLeft /></el-icon>
        <span>返回</span>
      </div>

    </div>
    <div v-if="show" class="intro">
      <p>您正在新建一个去中心化身份，同意使用条款后继续</p>
      <div class="clause">使用条款</div>
      <el-button type="primary" @click="handleAccept">接受</el-button>
    </div>
    <div v-else>
      <router-view/>
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
const handleAccept = () =>{
  router.push('/register/registerA')
  show.value = false
  console.log(show)
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
  .intro{
    margin: 20px auto;
    width: 600px;
    text-align: center;
    p{
      font-size: 20px;
      font-weight: bold;
      margin-bottom: 30px;
    }
    .clause{
      border: 1px #a0a0a6 solid;
      box-sizing: border-box;
      background-color: var(--bg-color, #fff);
      border-radius: 20px;
      height: 200px;
      overflow: hidden;
    }
    .el-button{
      margin-top: 20px;
    }
  }
}
</style>