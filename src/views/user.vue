<template>
	<div>
    <el-card shadow="hover">
      <template #header>
        <div class="clearfix">
          <div class="hh">认证个人信息</div>
        </div>
      </template>
      <div class="hhh">请注意：您的信息已加密我们无法获取也不会向已连接的网站泄露</div>
      <el-form label-width="100px" style="position: relative;top: 60px;height: 820px;">
        <el-row justify="center" align="middle">
          <el-col :span="6">
            <el-form-item label="用户名：">
              <el-input class="input" type="text" v-model="form.name"></el-input>
            </el-form-item>
            <el-form-item label="性别： ">
              <el-select v-model="value"  class="input" placeholder="Select" >
                <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                />
              </el-select>
            </el-form-item>
            <el-form-item label="身份证号：">
              <el-input class="input" type="text" v-model="form.id"></el-input>
            </el-form-item>
            <el-form-item label="年龄：">
              <el-input class="input" type="text" v-model="form.old"></el-input>
            </el-form-item>
            <el-form-item label="手机：">
              <el-input class="input" type="text" v-model="form.new"></el-input>
            </el-form-item>
            <el-form-item label="验证码：">
              <el-input class="input" v-model="form.code"></el-input>
            </el-form-item>
            <el-button  style="position: relative;top: -50px;left: 450px" >获取验证码</el-button>
            <el-form-item>
              <el-button style="position: relative;left: 85px" size="large" type="primary" @click="onSubmit">认证</el-button>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
    </el-card>

		<el-dialog title="裁剪图片" v-model="dialogVisible" width="600px">
			<vue-cropper
				ref="cropper"
				:src="imgSrc"
				:ready="cropImage"
				:zoom="cropImage"
				:cropmove="cropImage"
				style="width: 100%; height: 400px"
			></vue-cropper>

			<template #footer>
				<span class="dialog-footer">
					<el-button class="crop-demo-btn" type="primary"
						>选择图片
						<input class="crop-input" type="file" name="image" accept="image/*" @change="setImage" />
					</el-button>
					<el-button type="primary" @click="saveAvatar">上传并保存</el-button>
				</span>
			</template>
		</el-dialog>
	</div>
</template>

<script setup lang="ts" name="user">
import { reactive, ref } from 'vue';
import VueCropper from 'vue-cropperjs';
import 'cropperjs/dist/cropper.css';
import avatar from '../assets/img/img.jpg';

const name = localStorage.getItem('ms_username');
const form = reactive({
  name:'',
  id:'',
	age: '',
	mobile: '',
	code: ''
});
const value = ref('')

const options = [
  {
    value: '1',
    label: '男',
  },
  {
    value: '2',
    label: '女',
  },
]
const onSubmit = () => {};

const avatarImg = ref(avatar);
const imgSrc = ref('');
const cropImg = ref('');
const dialogVisible = ref(false);
const cropper: any = ref();

const showDialog = () => {
	dialogVisible.value = true;
	imgSrc.value = avatarImg.value;
};

const setImage = (e: any) => {
	const file = e.target.files[0];
	if (!file.type.includes('image/')) {
		return;
	}
	const reader = new FileReader();
	reader.onload = (event: any) => {
		dialogVisible.value = true;
		imgSrc.value = event.target.result;
		cropper.value && cropper.value.replace(event.target.result);
	};
	reader.readAsDataURL(file);
};

const cropImage = () => {
	cropImg.value = cropper.value.getCroppedCanvas().toDataURL();
};

const saveAvatar = () => {
	avatarImg.value = cropImg.value;
	dialogVisible.value = false;
};
</script>

<style scoped>
.info {
	text-align: center;
	padding: 35px 0;
}
.info-image {
	position: relative;
	margin: auto;
	width: 100px;
	height: 100px;
	background: #f8f8f8;
	border: 1px solid #eee;
	border-radius: 50px;
	overflow: hidden;
}

.info-edit {
	display: flex;
	justify-content: center;
	align-items: center;
	position: absolute;
	left: 0;
	top: 0;
	width: 100%;
	height: 100%;
	background: rgba(0, 0, 0, 0.5);
	opacity: 0;
	transition: opacity 0.3s ease;
}
.info-edit i {
	color: #eee;
	font-size: 25px;
}
.info-image:hover .info-edit {
	opacity: 1;
}
.info-name {
	margin: 15px 0 10px;
	font-size: 24px;
	font-weight: 500;
	color: #262626;
}
.crop-demo-btn {
	position: relative;
}
.crop-input {
	position: absolute;
	width: 100px;
	height: 40px;
	left: 0;
	top: 0;
	opacity: 0;
	cursor: pointer;
}
.hh{
  margin: 0 auto;
  width: 200px;
  font-size: 28px;
}
.input{
  width: 16rem;
}
.hhh{
  margin: 0 auto;
  width: 600px;
  font-size: 20px;
  color: red;
}


</style>
