<template>
  <el-card shadow="hover">
    <template #header>
      <div class="clearfix">
        <div class="hh">管理数据保存方式</div>
      </div>
    </template>
    <div>
      <div class="container">
        <el-form :model="form" label-width="180px">
          <el-row justify="center" align="middle">
            <el-col :span="6">
          <el-form-item label="您当前数据保存在" style="font-size: 22px">
            <el-input v-model="form.desc"  />
          </el-form-item>
          <el-form-item label="迁移到">
            <el-button type="primary" @click="onSubmit">Create</el-button>
            <el-button>Cancel</el-button>
          </el-form-item>
            </el-col>
          </el-row>
        </el-form>
      </div>
      <!-- 编辑弹出框 -->
    </div>
  </el-card>
</template>

<script setup lang="ts" name="basetable">
import { ref, reactive } from 'vue';


// do not use same name with ref
const form = reactive({
  desc: '',
})

const onSubmit = () => {
  console.log('submit!')
}



</script>

<style scoped>
.handle-box {
	margin-bottom: 20px;
}

.handle-select {
	width: 120px;
}

.handle-input {
	width: 300px;
}
.table {
	width: 100%;
	font-size: 14px;
}
.red {
	color: #F56C6C;
}
.mr10 {
	margin-right: 10px;
}
.table-td-thumb {
	display: block;
	margin: auto;
	width: 40px;
	height: 40px;
}
.hh{
  margin: 0 auto;
  width: 250px;
  font-size: 28px;
}
</style>
