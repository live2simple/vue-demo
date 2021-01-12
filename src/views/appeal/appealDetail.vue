<template>
  <div class="appeal-detail">
    <el-dialog
      title="诉求登记"
      :visible.sync="dialogVisible"
      width="85%">
      <el-form :model="dataInfo" :rules="rules" label-width="110px" >
        <!-- 基本信息 -->
        <el-card>
          <h4>企业基本信息</h4>
          <el-row>
            <el-col :span="12">
              <el-form-item label="企业名称：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="诉求来源：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="统一社会信用代码：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="企业法人：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="12">
              <el-form-item label="企业联系人：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="企业联系人电话：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="24">
              <el-form-item label="通讯地址：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <h4>企业诉求情况</h4>
          <el-form-item label="诉求标题目：" prop="name">
            <el-input v-model="dataInfo.name"></el-input>
          </el-form-item>
          <el-form-item label="诉求标题目：" prop="name">
            <el-input v-model="dataInfo.name"></el-input>
          </el-form-item>
          <el-form-item label="附件：" prop="name">
            <el-upload
              class="upload-demo"
              action="https://jsonplaceholder.typicode.com/posts/"
              multiple
              :limit="3">
              <el-button size="small" type="primary">上传附件</el-button>
            </el-upload>
          </el-form-item>
        </el-card>
        <!-- 评价 -->
        <el-card style="margin-top: 20px">
          <h4>评价情况</h4>
          <el-form-item label="综合评分：">
            <el-rate
              disabled
              show-score
              text-color="#ff9900">
            </el-rate>
          </el-form-item>
          <el-form-item label="评价意见：">
            <el-input v-text="dataInfo.name"></el-input>
          </el-form-item>
        </el-card>

        <!-- 受理 -->
        <el-card style="margin-top: 20px">
          <h4>受理情况</h4>
          <el-row>
            <el-col :span="8">
              <el-form-item label="受理交办人：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="受理时间：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="紧急程度：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row>
            <el-col :span="24">
              <el-form-item label="选择办理单位：" prop="name">
                <el-button type="primary">办理单位</el-button><span>(可多选)</span>
              </el-form-item>
            </el-col>
          </el-row>
          <el-form-item label="受理意见：" prop="name">
                <el-input v-model="dataInfo.name"></el-input>
          </el-form-item>
          <el-form-item label="附件：" prop="name">
            <el-upload
              class="upload-demo"
              action="https://jsonplaceholder.typicode.com/posts/"
              multiple
              :limit="3">
              <el-button size="small" type="primary">上传附件</el-button>
            </el-upload>
          </el-form-item>
        </el-card>

        <!-- 审核 -->
        <el-card style="margin-top: 20px">
          <h4>审核情况</h4>
          <el-table border :data="dataList">
            <el-table-column label="操作者" align="center" prop="name"></el-table-column>
            <el-table-column label="操作时间" align="center" prop="name"></el-table-column>
            <el-table-column label="环节" align="center" prop="name"></el-table-column>
            <el-table-column label="处理意见" align="center" prop="name"></el-table-column>
          </el-table>
        </el-card>
      </el-form>

      <div class="dialog-footer">
        <el-button type="primary">提交</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      dialogVisible: false,
      dialogStatus: 'add',
      dataInfo: {
        name: ''
      },
      rules: {},
      dataList: []
    }
  },
  methods: {
    init(status) {
      this.dialogVisible = true
      this.dialogStatus = status

      this.$nextTick(() => {

      })
    }
  }
}
</script>

<style lang="scss" scoped>
.appeal-detail {
  h4 {
    margin: 10px 0;
    font-size: 18px;
    color: #333;
    font-weight: 500;
  }
  .dialog-footer {
    text-align: right;
    margin-top: 20px;
  }
}
  
</style>