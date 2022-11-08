<template>
  <div class="table-box">
    <!-- 标题 -->
    <div class="title">
      <h2>最简单的 CRUD Demo</h2>
    </div>
    <!-- query -->
    <div class="query-box">
      <el-input v-model="queryInput" placeholder="请输入姓名搜索🔍" />
      <el-button type="primary" @click="handleAdd">增加</el-button>
    </div>
    <!-- table -->
    <el-table 
      ref="multipleTableRef"
      :data="tableData"
      style="width: 100%"
      @selection-change="handleSelectionChange"
      border>
      <el-table-column type="selection" width="60" />
      <el-table-column prop="name" label="姓名" width="120" />
      <el-table-column prop="email" label="邮箱" width="120" />
      <el-table-column prop="phone" label="手机" width="120" />
      <el-table-column prop="state" label="状态" width="120" />
      <el-table-column prop="address" label="地址" width="300"/>

      <!-- 操作侧栏 -->
      <el-table-column fixed="right" label="操作" width="120">
        <template #default="scope">
          <el-button link type="primary" size="small" 
          @click="handleRowDelete(scope.row)" style="color:red;" >删除</el-button>
          <el-button link type="primary" size="small">编辑</el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- dialog -->   
    <el-dialog v-model="dialogFormVisible" :title="dialogType === 'add' ? '新增用户' : '编辑'" >
      <el-form :model="tableForm">
        <el-form-item label="姓名" :label-width="80">
          <el-input v-model="tableForm.name" autocomplete="off" />
        </el-form-item>
        <el-form-item label="邮箱" :label-width="80">
          <el-input v-model="tableForm.email" autocomplete="off" />
        </el-form-item>
        <el-form-item label="手机" :label-width="80">
          <el-input v-model="tableForm.phone" autocomplete="off" />
        </el-form-item>
        <el-form-item label="状态" :label-width="80">
          <el-input v-model="tableForm.state" autocomplete="off" />
        </el-form-item>
        <el-form-item label="地址" :label-width="80">
          <el-input v-model="tableForm.adress" autocomplete="off" />
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button type="primary" @click="dialogConfirm" >
            确认
          </el-button>
        </span>
      </template>
    </el-dialog>
  </div>
</template>

<script setup>
import { ref } from 'vue'
//  数据
let queryInput = $ref("")
let tableData = $ref([
  {
    id:'1',
    name: 'Tom',
    email:'123@qq.com',
    state: 'California',
    phone:'15572888613',
    address: 'No. 189, Grove St, Los Angeles',
  },
])
let multipleSelection = $ref([])
let dialogFormVisible = $ref(false)
let tableForm = $ref({
  name: '',
  email: '',
  phone: '',
  state: '',
  adress: ''
})
let dialogType = $ref("add")

// 方法
const handleRowDelete = ({id}) => {
  // 1. 通过id获取到条目对应的索引值
  let index = tableData.findIndex(item => item.id === id)
  // 2. 通过索引值进行删除对应条目
  tableData.splice(index, 1)
}

const handleSelectionChange = (val) => {
  multipleSelection = val
}

const handleAdd = () => {
  dialogFormVisible = true   // 打开弹窗
  tableForm = {}
}

const dialogConfirm = () => {
  dialogFormVisible = false   // 关闭弹窗
  // 1. 拿到数据

  // 2. 添加到table
  tableData.push({
    id:(tableData.length + 1).toString(),
    ...tableForm
  })
  console.log(tableData);
}
</script>

<style scoped>
.table-box{
  width: 800px;
  margin: 100px auto;
}
.title{
  text-align: center;
}

.query-box{
  display: flex;
  justify-content:space-between ;
  margin-bottom: 20px;
}
.el-input{
  width: 200px;
}
</style>
