<template>
	<div class="container">
		<el-tabs v-model="message">
			<el-tab-pane :label="`已验证属性的消息`" name="first">
				<el-table :data="state.unread" :show-header="false" style="width: 100%">
					<el-table-column>
						<template #default="scope">
							<span class="message-title">{{ scope.row.title }}</span>
						</template>
					</el-table-column>
					<el-table-column prop="date" width="180"></el-table-column>
					<el-table-column width="120">
						<template #default="scope">
							<el-button  @click="forward">同意，前往</el-button>
              <el-button type="danger" style="margin-top: 10px" @click="clearData1">拒绝</el-button>
						</template>
					</el-table-column>
				</el-table>

			</el-tab-pane>
			<el-tab-pane :label="`未验证属性的消息`" name="second">
				<template v-if="message === 'second'">
					<el-table :data="state.read" :show-header="false" style="width: 100%">
						<el-table-column>
							<template #default="scope">
								<span class="message-title">{{ scope.row.title }}</span>
							</template>
						</el-table-column>
						<el-table-column prop="date" width="150"></el-table-column>
						<el-table-column width="120">
							<template #default="scope">
								<el-button  @click="forward">前往添加</el-button>
                <el-button type="danger" style="margin-top: 10px" @click="clearData2()" >拒绝</el-button>
							</template>
						</el-table-column>
					</el-table>

				</template>
			</el-tab-pane>
		</el-tabs>
	</div>
</template>

<script setup lang="ts" name="tabs">
import { ref, reactive } from 'vue';
import { useRouter } from 'vue-router';
import {VXETable} from "vxe-table";
const router = useRouter();
const message = ref('first');
const state = reactive({
	unread: [
		{
			date: '2018-04-19 20:00:00',
			title: 'www.baidu.com请求您的属性'
		},
	],
	read: [
		{
			title: 'www.google.com请求访问您的属性'
		}
	],
	recycle: [
		{
			date: '2018-04-19 20:00:00',
			title: '【系统通知】该系统将于今晚凌晨2点到5点进行升级维护'
		}
	]
});
const forward = () => {
  router.push('/editinfo')
}
const clearData1 = () =>{
  state.unread[0] = {
    date: '2018-04-19 20:00:00',
    title: ''
  }
}


const clearData2 = () =>{
  state.read[0] = {
    title: ''
  }
}
</script>

<style>
.message-title {
	cursor: pointer;
}
.handle-row {
	margin-top: 30px;
}
</style>
