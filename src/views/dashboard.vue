<template>
	<div>
		<el-row :gutter="20" justify="center">
			<el-col :span="24">
        <el-card shadow="hover" >

          <div class="img-box" style="text-align: center">
            <img src="../assets/img/logo_transparent.png" style="width: 300px;height: 300px" alt="myId"/>
          </div>
          <el-row :gutter="16" justify="center">
            <el-col :span="6">
              <el-card shadow="hover" style="border: 2px #dadae5 solid;;border-radius: 10px;background-color: white">
                <el-container>
                  <el-main>
                    <el-row justify="center">
                      <el-col :span="15">
                        <p style="font-size: 28px;color: #606266 ">认证基本信息</p>
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
            <el-col :span="6">
              <el-card shadow="hover" style="border: 2px #dadae5 solid;;border-radius: 10px;background-color: white">
                <el-container>
                  <el-main>
                    <el-row justify="center">
                      <el-col :span="15">
                        <p style="font-size: 28px;color: #606266 ">编辑个人信息</p>
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
            <el-col :span="6">
              <el-card shadow="hover" style="border: 2px #dadae5 solid;;border-radius: 10px;background-color: white">
                <el-container>
                  <el-main>
                    <el-row justify="center">
                      <el-col :span="18">
                        <p style="font-size: 28px;color: #606266 ">管理已连接的网站</p>
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
            <el-col :span="6">
              <el-card shadow="hover" style="border: 2px #dadae5 solid;;border-radius: 10px;background-color: white">
                <el-container>
                  <el-main>
                    <el-row justify="center">
                      <el-col :span="18">
                        <p style="font-size: 28px;color: #606266">管理您的数据</p>
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
          </el-row>
        </el-card>
			</el-col>
		</el-row>

	</div>
</template>

<script setup lang="ts" name="dashboard">
import Schart from 'vue-schart';
import { reactive } from 'vue';
import imgurl from '../assets/img/img.jpg';

const name = localStorage.getItem('ms_username');
const role: string = name === 'admin' ? '超级管理员' : '普通用户';


</script>

<style scoped>
.el-row {
	margin-bottom: 20px;
}

.grid-content {
	display: flex;
	align-items: center;
	height: 100px;
}

.grid-cont-right {
	flex: 1;
	text-align: center;
	font-size: 14px;
	color: #999;
}

.grid-num {
	font-size: 30px;
	font-weight: bold;
}

.grid-con-icon {
	font-size: 50px;
	width: 100px;
	height: 100px;
	text-align: center;
	line-height: 100px;
	color: #fff;
}

.grid-con-1 .grid-con-icon {
	background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
	color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
	background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
	color: rgb(100, 213, 114);
}

.grid-con-3 .grid-con-icon {
	background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
	color: rgb(242, 94, 67);
}
.user{
  border-bottom: 2px solid #ccc;
}
.user-info {
	display: flex;
	align-items: center;
	padding-bottom: 20px;
	margin-bottom: 20px;
}

.user-info-cont {
	padding-left: 50px;
	flex: 1;
	font-size: 14px;
	color: #999;
}

.user-info-cont div:first-child {
	font-size: 30px;
	color: #222;
}

.user-info-list {
	font-size: 14px;
	color: #999;
	line-height: 25px;
}

.user-info-list span {
	margin-left: 70px;
}

.mgb20 {
	margin-bottom: 20px;
}

.todo-item {
	font-size: 14px;
}

.todo-item-del {
	text-decoration: line-through;
	color: #999;
}

.schart {
	width: 100%;
	height: 300px;
}

</style>
