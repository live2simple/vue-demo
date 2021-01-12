<template>
  <el-dialog  :visible.sync="dialogVisible" width="75%">
    <div class="appeal-form">
      <h4>濠江区政企直通车收文呈批表</h4>
      <div class="appeal-header">
        <div>
          <span class="header-label">来文文号：</span>
          <span class="header-value">濠江区政企直通车[2021]0001号</span>
        </div>
        <div>
          <span class="header-label">密级：</span>
          <span class="header-value">普通</span>
        </div>
        <div>
          <span class="header-label">紧急程度：</span>
          <span class="header-value">普通</span>
        </div>
      </div>
      <table class="table-wrapper" border="1" bordercolor="red">
        <tr class="table-row">
          <td class="table-label">来文标题</td>
          <td class="table-value" colspan="3">111</td>
        </tr>
        <tr class="table-row">
          <td class="table-label">来文日期</td>
          <td class="table-value" colspan="3">111</td>
        </tr>
        <tr class="table-row">
          <td class="table-label" style="height: 120px">来文内容</td>
          <td class="table-value" colspan="3" style="min-height: 150px;">111</td>
        </tr>
        <tr class="table-row">
          <td class="table-label">企业名称</td>
          <td class="table-value" colspan="3">111</td>
        </tr>
        <tr class="table-row">
          <td class="table-label">企业联系人</td>
          <td class="table-value">111</td>
          <td class="table-label">联系电话</td>
          <td class="table-value">222</td>
        </tr>
        <tr class="table-row">
          <td class="table-label" style="height: 120px">受理意见</td>
          <td class="table-value">111</td>
          <td class="table-label">副组长拟办意见</td>
          <td class="table-value">222</td>
        </tr>
        <tr class="table-row">
          <td class="table-label" style="height: 120px">组长意见</td>
          <td class="table-value">111</td>
          <td class="table-label">办公室副主任批示</td>
          <td class="table-value">222</td>
        </tr>
      </table>

      <el-form :model="dataInfo" label-width="110px">
        <el-form-item label="受理意见：">
          <el-input type="textarea" :rows="3"></el-input>
        </el-form-item>
        <el-form-item label="附件：">
          <el-upload
              class="upload-demo"
              action="https://jsonplaceholder.typicode.com/posts/"
              multiple
              :limit="3">
              <el-button size="small" type="primary">上传附件</el-button>
            </el-upload>
        </el-form-item>
      </el-form>

      <div class="dialog-footer">
        <el-button>取消</el-button>
        <el-button type="primary">提交</el-button>
      </div>
    </div>
  </el-dialog>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
    };
  },
  methods: {
    init() {
      this.dialogVisible = true;

      this.$nextTick(() => {});
    },
  },
};
</script>

<style lang="scss" scoped>
.appeal-form {
  padding: 0 20px 20px 20px;
  h4 {
    margin-bottom: 20px;
    margin-top: 0;
    text-align: center;
    color: red;
    font-size: 24px;
    font-weight: 600;
  }
  .appeal-header {
    font-size: 14px;
    margin-bottom: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .header-label {
      color: red;
    }
    .header-value {
      // font-weight: 600;
      color: #000;
    }
  }
  .table-wrapper {
    border-collapse: collapse;
    // background: #ddd;
    // border-color: red;
    width: 100%;
    margin-bottom: 20px;
    .table-label {
      min-width: 100px;
      min-height: 30px;
      line-height: 30px;
      text-align: center;
      color: red;
    }
    .table-value {
      width: 40%;
      background: #fff;
      line-height: 30px;
      padding-left: 10px;
    }
  }
  .dialog-footer {
    text-align: right;
  }
}
</style>