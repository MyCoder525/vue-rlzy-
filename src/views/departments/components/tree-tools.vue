<template>
  <!-- 头部和列表组件 -->
  <!-- 用了一个行列布局 -->
  <el-row type="flex" style="width:100%;" justify="space-between">
    <el-col>
      <span>{{ treeData.name }}</span>
    </el-col>
    <el-col :span="8">
      <el-row type="flex" justify="space-between">
        <!-- 两个内容 -->
        <el-col>{{ treeData.manager }}</el-col>
        <el-col>
          <!-- 下拉菜单 element -->
          <el-dropdown style="margin-left:10px;" @command="clickItem">
            <span>
              操作<i class="el-icon-arrow-down" />
            </span>
            <!-- 下拉菜单 -->
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item command="add">添加部门</el-dropdown-item>
              <el-dropdown-item v-if="!isRoot" command="edit">编辑部门</el-dropdown-item>
              <el-dropdown-item v-if="!isRoot" command="del">删除部门</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-col>
      </el-row>
    </el-col>
  </el-row>
</template>

<script>
import { delDepartments } from '@/api/departments.js'
export default {
  name: 'TreeTool',
  // 接受父组件的传值 循环树形结构数据
  props: {
    treeData: {
      type: Object,
      default: () => ({})
    },
    isRoot: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    async clickItem(type) {
      if (type === 'add') {
        // 接受父组件传的自定义事件 把treeData的每一行数据传给父组件接受
        this.$emit('handlerAddDept', this.treeData)
        // 添加逻辑
      } else if (type === 'edit') {
        // 接受父组件传的自定义事件  把treeData的每一行数据传给父组件接受
        this.$emit('handlerEditDept', this.treeData)
        // 编辑逻辑
      } else {
        // 删除逻辑
        await this.$confirm('确认删除？')
        await delDepartments(this.treeData.id)
        this.$message.success('删除成功')
        this.$emit('getDepartments')
      }
    }
  }
}
</script>

<style>
</style>