<template>
  <div class="select-unit">
    <el-dialog title="选择办理单位" :visible.sync="dialogVisible" width="30%">
      <el-table border :data="tableData" @selection-change="handleSelect">
        <el-table-column type="selection" width="55px"></el-table-column>
        <el-table-column label="单位名称" prop="unitName" align="center"></el-table-column>
      </el-table>

      <div class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="handleConfirm">确定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { getAction } from "@/api/manage";
export default {
  data() {
    return {
      dialogVisible: false,
      tableData: [],
      selections: []
    };
  },
  methods: {
    // 树转数组扁平化结构
    toArray(data, parId) {
      const result = [];
      const loop = (item, parId) => {
        // debugger
        let child = item.children;
        result.push({
          id: item.id,
          unitName: item.label,
          parentId: parId,
          isLeaf: item.isLeaf,
        });
        if (child) {
          for (let i = 0; i < child.length; i++) {
            loop(child[i], item.id);
          }
        }
      };
      loop(data, parId);
      return result;
    },
    init() {
      this.dialogVisible = true

      this.$nextTick(() => {
        this.getTreeData()
      })
    },
    getTreeData() {
      getAction('/sys/org/getTreeData').then(res => {
        if (res.code === 200) {
          console.log(res.data)
          this.tableData = this.toArray(res.data[0])
        }
      })
    },
    handleSelect(data) {
      this.selections = data
    },
    handleConfirm() {
      this.$emit('handleSetUnit', this.selections)
      this.dialogVisible = false
    }
  },
  mounted() {
    // this.getTreeData()
  }
};
</script>

<style lang="scss" scoped>
  .select-unit {
    .dialog-footer {
      margin: 10px 0;
      text-align: right;
    }
  }
</style>