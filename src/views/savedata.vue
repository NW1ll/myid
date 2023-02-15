<template>
  <el-card shadow="hover">
    <template #header>
      <div class="clearfix">
        <div class="hh">编辑个人信息</div>
      </div>
    </template>
    <div class="container">
      <vxe-switch  v-model="switchValue" close-label="打开编辑"  open-label="关闭编辑" @change="switchEvent"></vxe-switch>
      <vxe-button @click="insertEvent()">新增</vxe-button>
      <vxe-grid ref="xTable"  v-bind="gridOptions" >
        <template #prop_default="{ row, column }">
<!--          后续动态遍历-->
          <el-link type="primary" v-if="row.prop === '邮箱'" href="/www.baidu.com">{{row.prop}}</el-link>
          <el-link type="primary" v-if="row.prop === '爱好'" href="/www.baidu.com">爱好</el-link>
          <el-link type="primary" v-if="row.prop === '简介'" href="/www.baidu.com">简介</el-link>
        </template>
        <template #content_edit="{ row, column }">
          <vxe-input v-model="row.content" ></vxe-input>
        </template>
        <template #default="{ row }">
          <vxe-button status="danger" content="删除" @click="deleteRowEvent(row)"></vxe-button>
        </template>
      </vxe-grid>
    </div>
  </el-card>
</template>

<script  lang="ts" name="savedata">
import { ref, reactive,defineComponent } from 'vue';
import {VxeGridInstance,  VxeGridProps, VXETable } from 'vxe-table'
  export default defineComponent({
    setup () {
      const xTable =ref<VxeGridInstance>()
      const gridOptions = reactive<VxeGridProps>({
        border: true,
        stripe: true,
        showFooter: true,
        height: 500,
        tooltipConfig: {},
        exportConfig: {},
        menuConfig: {},
        columnConfig: {
          resizable: true
        },
        toolbarConfig: {
          export: true,
          zoom: true
        },
        editConfig: {
          trigger: 'click',
          mode: 'row',
          enabled:false,
        },
        columns: [
          { field: 'prop', title: '个人属性', slots: { default: 'prop_default' } },
          { field: 'createDate', title: '创建日期'  },
          { field: 'lastDate', title: '最后修改日期' },
          { field: 'content', title: '属性内容', editRender: {}, slots: { edit: 'content_edit' } },
          { title: '操作',  slots: { default: 'default' },align:'center'}
        ],
        data: [
          {  prop: '邮箱', createDate: 'Develop',  content: 28, lastDate: '2021-03-13' },
          {  prop: '爱好', createDate: 'Test',  content: 22, lastDate: '2021-01-05' },
          {  prop: '简介', createDate: 'PM',  content: 32, lastDate: '2021-04-13' },
        ],
      })
      const insertEvent = async (row: any) => {
        const $table = xTable.value
        const record = {
          prop: '电话',
        }
        const { row: newRow } = await $table!.insertAt(record, row)
        await $table!.setEditCell(newRow, 'prop')
      }
      const switchValue = ref(false);
      const deleteRowEvent = async (row: any) => {
        const $table = xTable.value
        const type = await VXETable.modal.confirm('您确定要删除该数据?')
        if (type === 'confirm') {
          await $table!.remove(row)
        }
      }
      const switchEvent = (event:any) =>{
        if(event.value){
          gridOptions.editConfig = {
            trigger: 'click',
            mode: 'row',
            enabled:true,
          }
        }else {
          gridOptions.editConfig = {
            trigger: 'click',
            mode: 'row',
            enabled:false,
          }

        }
      }

      return {
        insertEvent,
        gridOptions,
        switchValue,
        deleteRowEvent,
        switchEvent,
        xTable
      }
    }
  })



</script>

<style scoped>
.handle-box {
    display: flex;
    margin-bottom: 20px;
}

.table {
    width: 100%;
    font-size: 14px;
}
.mr10 {
    margin-right: 10px;
}
.hh{
  margin: 0 auto;
  width: 250px;
  font-size: 28px;
}
</style>
