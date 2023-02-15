<template>
	<div class="container">
		<el-tabs v-model="message">
			<el-tab-pane :label="`已验证属性的消息`" name="first">
				<vxe-table ref="xTable" :data="state1.unread" :show-header="false" >
					<vxe-column>
						<template #default="{row}">
							<span class="message-title">{{ row.title }}</span>
						</template>
					</vxe-column>
					<vxe-column width="120">
						<template #default="{row}">
							<el-button  @click="forward">同意，前往</el-button>
              <el-button type="danger" style="margin-top: 10px" @click="deleteRowEvent(row)">拒绝</el-button>
						</template>
					</vxe-column>
				</vxe-table>
			</el-tab-pane>

			<el-tab-pane :label="`未验证属性的消息`" name="second">
				<template v-if="message === 'second'">
					<vxe-table ref="xTable2" :data="state2.read" :show-header="false" style="width: 100%">
						<vxe-column>
							<template #default="{row}">
								<span class="message-title">{{ row.title }}</span>
							</template>
						</vxe-column>
						<vxe-column width="120">
							<template #default="{row}">
								<el-button  @click="forward">前往添加</el-button>
                <el-button type="danger" style="margin-top: 10px" @click="deleteRowEvent2(row)">拒绝</el-button>
							</template>
						</vxe-column>
					</vxe-table>
				</template>
			</el-tab-pane>
		</el-tabs>
	</div>
</template>

<script  lang="ts" name="savedata">
import { ref, reactive,defineComponent } from 'vue';
import {VxeGridInstance,  VxeGridProps, VXETable } from 'vxe-table'
import {useRouter} from "vue-router";
export default defineComponent({
  setup () {
    const xTable =ref<VxeGridInstance>()
    const xTable2 =ref<VxeGridInstance>()
    const message = ref('first');

    const deleteRowEvent = async (row: any) => {
      const $table = xTable.value
      const type = await VXETable.modal.confirm('您确定要删除该数据?')
      if (type === 'confirm') {
        await $table!.remove(row)
      }
    }

    const deleteRowEvent2 = async (row: any) => {
      const $table1 = xTable2.value
      const type = await VXETable.modal.confirm('您确定要删除该数据?')
      if (type === 'confirm') {
        await $table1!.remove(row)
      }
    }

    const router = useRouter();
    const state1 = reactive({
      unread: [
        {
          title: 'www.baidu.com请求您的属性'
        }
      ]
    });

    const state2 = reactive({
      read: [
        {
          title: 'www.google.com请求您的属性'
        }
      ]
    });

    const forward = () => {
      router.push('/editinfo')
    }

    return {
      deleteRowEvent,
      deleteRowEvent2,
      state1,
      state2,
      forward,
      xTable,
      xTable2,
      message
    }
  }
})



</script>

<style>
.message-title {
	cursor: pointer;
}
.handle-row {
	margin-top: 30px;
}
</style>
