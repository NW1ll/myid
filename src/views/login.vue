<template>
	<div class="login-wrap">
    <div class="img-box">
      <img src="../assets/img/logo_transparent.png" alt="myId" />
      <span>MyId:由您自己掌握的Web3分布式身份</span>
    </div>
		<div class="ms-login">
      <div class="links">
        <div class="link">
          <p>使用助记词登陆现有身份</p>
          <router-link class="aa"  :to="{name:'loginDetail'}">登陆</router-link>
        </div>
        <div class="link">
          <p>使用全新的助记词创建新身份</p>
          <router-link class="aa"  to="/register">创建</router-link>
        </div>
      </div>
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
.login-wrap {
	position: relative;
	width: 100%;
	height: 100%;
	background-image: url(../assets/img/login.svg);
	background-size: 100%;
  .ms-login {
    position: relative;
    top: 20px;
    margin: 0 auto;
    width: 500px;
    .links{
      display: flex;
      justify-content: space-between;
      .link {
        text-align: center;
        background-color: #f0f2f5;
        border: 1px solid #f0f2f5;
        width: 200px;
        height: 100px;
        margin: 0 20px;
        box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;
        .aa{
          color: #2d8cf0;
          display: block;
        }
        p{
          padding: 5px 5px

        }
      }
    }
    .router-view{
      overflow: hidden;
      height: 500px;
      box-shadow:0 1px 4px rgba(0, 0, 0, 0.3), 0 0 20px rgba(0, 0, 0, 0.1) inset;
    }
  }
}
.ms-title {
	width: 100%;
	line-height: 50px;
	text-align: center;
	font-size: 20px;
	color: #fff;
	border-bottom: 1px solid #ddd;
}
.ms-content {
	padding: 30px 30px;
}
.login-btn {
	text-align: center;
}
.login-btn button {
	width: 100%;
	height: 36px;
	margin-bottom: 10px;
}
.login-tips {
	font-size: 12px;
	line-height: 30px;
	color: #fff;
}

.img-box {
  text-align: center;
  img {
    padding-top: 200px;
    vertical-align: middle;
    height: 200px;
    width: 200px;
  }
  span {
    display: block;
    color: #4782ad;
    font-size: 36px;
    font-family: Avenir, "Helvetica Neue", Arial, Helvetica, sans-serif;
    font-weight: 600;
  }
}


.active {
  font-size: 20px;
  font-weight: bold;
}
</style>
