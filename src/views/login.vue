<template>
	<div class="login-wrap">
    <div class="img-box">
      <img src="../assets/img/logo_transparent.png" alt="myId" />
      <span>MyId:由您自己掌握的Web3分布式身份</span>
    </div>
		<div class="ms-login">
<!--      <div class="links">-->
<!--        <div class="link">-->
<!--          <p>使用助记词登陆现有身份</p>-->
<!--          <router-link class="aa"  :to="{name:'loginDetail'}">登陆</router-link>-->
<!--        </div>-->
<!--        <div class="link">-->
<!--          <p>使用全新的助记词创建新身份</p>-->
<!--          <router-link class="aa"  to="/register">创建</router-link>-->
<!--        </div>-->
<!--      </div>-->
      <el-row :gutter="16" justify="center">
        <el-col :span="12">
          <el-card shadow="hover" style="border: 2px #dadae5 solid;;border-radius: 10px;background-color: #f6f5f5">
            <el-container>
              <el-main>
                <el-row justify="center">
                  <el-col :span="18">
                    <el-icon style="font-size: 30px;position: relative;left: 80px"><Discount /></el-icon>
                    <p style="font-size: 18px;color: #606266 ">使用助记词登陆现有身份。</p>
                  </el-col>
                </el-row>
              </el-main>
              <el-footer>
                <el-row justify="center">
                  <el-col :span="15">
                    <el-button type="primary" round style="width: 160px;margin-top: 20px" @click="handleLogin">登陆</el-button>
                  </el-col>
                </el-row>
              </el-footer>
            </el-container>
          </el-card>
        </el-col>
        <el-col :span="12">
          <el-card shadow="hover" style="border: 2px #dadae5 solid;;border-radius: 10px;background-color: #f6f5f5">
            <el-container>
              <el-main>
                <el-row justify="center">
                  <el-col :span="20">
                    <el-icon style="font-size: 30px;position: relative;left: 92px"><Plus /></el-icon>
                    <p style="font-size: 18px;color: #606266 ">使用全新的助记词创建新身份。</p>
                  </el-col>
                </el-row>
              </el-main>
              <el-footer>
                <el-row justify="center">
                  <el-col :span="15">
                    <el-button type="primary" round style="width: 160px;margin-top: 20px" @click="handleRegister">注册</el-button>
                  </el-col>
                </el-row>
                <!--                <router-link class="aa"  :to="{name:'loginDetail'}">登陆</router-link>-->
              </el-footer>
            </el-container>
          </el-card>
        </el-col>
      </el-row>
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

const handleLogin = () => {
  router.push('/detail')
}
const handleRegister = () => {
  router.push('/register')
}

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
    top: 50px;
    margin: 0 auto;
    width: 800px;
    .aa{
      color: #2d8cf0;
      display: block;
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
    padding-top: 100px;
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
