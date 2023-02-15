<template>
  <el-card shadow="hover">
    <template #header>
      <div class="clearfix">
        <div class="hh">管理已连接的网站</div>
      </div>
    </template>
    <div class="container">
      <vxe-grid ref="xTable"  v-bind="gridOptions" >
        <template #prop_default="{ row, column }">
          <!--          后续动态遍历-->
          <el-link type="primary" v-if="row.prop === 'YouTube'" href="/www.youtube.com">{{row.prop}}</el-link>
          <el-link type="primary" v-if="row.prop === '谷歌'" href="/www.baidu.com">{{row.prop}}</el-link>
          <el-link type="primary" v-if="row.prop === 'Twitter'" href="/www.baidu.com">{{row.prop}}</el-link>
        </template>
        <template #content_edit="{ row, column }">
          <vxe-input v-model="row.content" ></vxe-input>
        </template>
        <template #default="{ row }">
          <vxe-button status="primary" class-name="aa" content="管理属性" ></vxe-button>
          <vxe-button status="primary" content="取消授权" @click="deleteRowEvent(row) "></vxe-button>
        </template>
      </vxe-grid>
    </div>
  </el-card>
</template>

<script  lang="ts" >
import {defineComponent, reactive, ref} from 'vue';
import {VxeGridInstance, VxeGridProps, VXETable} from "vxe-table";
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
        { field: 'website', title: '网站', slots: { default: 'prop_default' } },
        { field: 'did', title: 'did', editRender: {}, slots: { edit: 'content_edit' } },
        { title: '操作',  slots: { default: 'default' },align:'center'}
      ],
      data: [
        {  prop: 'YouTube', did: 'Develop'},
        {  prop: '谷歌', did: 'Test' },
        {  prop: 'Twitter', did: 'PM' },
      ],
    })
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
      gridOptions,
      xTable,
      deleteRowEvent,
      switchEvent
    }
  }
})

</script>

<style scoped>
.hh{
  margin: 0 auto;
  width: 250px;
  font-size: 28px;
}

</style>
