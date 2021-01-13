<template>
  <div class="appeal-detail">
    <el-dialog title="诉求登记" :visible.sync="dialogVisible" width="85%">
      <el-form
        :model="dataInfo"
        :rules="rules"
        label-width="140px"
        ref="dataInfo"
      >
        <!-- 基本信息 -->
        <el-card>
          <h4>企业基本信息</h4>
          <el-row :gutter="20">
            <el-col :span="12">
              <el-form-item label="企业名称：" prop="epName">
                <el-input v-model="dataInfo.epName"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="诉求来源：" prop="password">
                <el-select v-model="dataInfo.password" style="width: 100%">
                  <el-option
                    label="市政企直通车专班移交"
                    value="市政企直通车专班移交"
                  ></el-option>
                  <el-option
                    label="“12345”热点移交"
                    value="“12345”热点移交"
                  ></el-option>
                  <el-option
                    label="“粤商通”渠道移交"
                    value="“粤商通”渠道移交"
                  ></el-option>
                  <el-option
                    label="“今日濠江”微信公众号移交"
                    value="“今日濠江”微信公众号移交"
                  ></el-option>
                </el-select>
                <!-- <el-input v-model="dataInfo.password"></el-input> -->
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="12">
              <el-form-item
                label="统一社会信用代码："
                prop="unifiedSocialCreditCode"
              >
                <el-input v-model="dataInfo.unifiedSocialCreditCode"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="企业法人：" prop="legalPerson">
                <el-input v-model="dataInfo.legalPerson"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="12">
              <el-form-item label="企业联系人：" prop="linkman">
                <el-input v-model="dataInfo.linkman"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="企业联系人电话：" prop="contactNumber">
                <el-input v-model="dataInfo.contactNumber"></el-input>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row :gutter="20">
            <el-col :span="24">
              <el-form-item label="通讯地址：" prop="address">
                <el-input v-model="dataInfo.address"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <h4>企业诉求情况</h4>
          <el-form-item label="诉求标题：" prop="title">
            <el-input v-model="dataInfo.title"></el-input>
          </el-form-item>
          <el-form-item label="诉求内容：" prop="content">
            <el-input v-model="dataInfo.content"></el-input>
          </el-form-item>
          <el-form-item label="附件：" prop="name">
            <el-upload
              class="upload-demo"
              action="/zjmzxfzhl/accessory/info/uploadFile"
              multiple
              :headers="fileHeader"
              :on-success="handleGetFile"
              :file-list="fileList"
            >
              <el-button size="small" type="primary">上传附件</el-button>
            </el-upload>
          </el-form-item>
        </el-card>
        <!-- 评价 -->
        <!-- <el-card style="margin-top: 20px">
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
        </el-card> -->

        <!-- 受理 -->
        <el-card style="margin-top: 20px">
          <h4>受理情况</h4>
          <el-row :gutter="20">
            <el-col :span="8">
              <el-form-item label="受理交办人：">
                <el-input v-text="$store.state.user.name"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="受理时间：">
                <!-- <el-date-picker type="datetime" v-model="dataInfo.now" disabled></el-date-picker> -->
                <el-input v-text="dataInfo.now"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="8">
              <el-form-item label="紧急程度：" prop="name">
                <el-select v-model="dataInfo.whetherUrgent">
                  <el-option label="急件" :value="true"></el-option>
                  <el-option
                    label="要件，非急件"
                    :value="false"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- </el-row>
          <el-row :gutter="20"> -->
            <el-col :span="24">
              <el-form-item label="选择办理单位：" prop="name">
                <el-button type="primary" @click="handleSelectUnit"
                  >办理单位</el-button
                ><span>(可多选)</span>
              </el-form-item>

              <div class="unit-wrapper" v-for="el in unitList" :key="el.orgId">
                <div class="unit-item">
                  <span>办理单位：</span>
                  <span>{{ el.orgName }}</span>
                </div>
                <div class="unit-item">
                  <span>办理负责人：</span>
                  <span>{{ el.dealPersonLiable }}</span>
                </div>
                <div class="unit-item">
                  <span>联系方式：</span>
                  <span>{{ el.contactNumber }}</span>
                </div>
              </div>
            </el-col>
          </el-row>
          <el-form-item label="受理意见：" prop="name">
            <el-input
              v-model="dataInfo.message"
              type="textarea"
              :rows="3"
            ></el-input>
          </el-form-item>
          <el-form-item label="附件：" prop="name">
            <el-upload
              class="upload-demo"
              action="/zjmzxfzhl/accessory/info/uploadFile"
              multiple
              :headers="fileHeader"
              :on-success="handleGetFile2"
              :file-list="appealList"
            >
              <el-button size="small" type="primary">上传附件</el-button>
            </el-upload>
          </el-form-item>
        </el-card>

        <!-- 审核 -->
        <!-- <el-card style="margin-top: 20px">
          <h4>审核情况</h4>
          <el-table border :data="dataList">
            <el-table-column label="操作者" align="center" prop="name"></el-table-column>
            <el-table-column label="操作时间" align="center" prop="name"></el-table-column>
            <el-table-column label="环节" align="center" prop="name"></el-table-column>
            <el-table-column label="处理意见" align="center" prop="name"></el-table-column>
          </el-table>
        </el-card> -->
      </el-form>

      <div class="dialog-footer">
        <el-button type="primary" @click="handleSubmit">提交</el-button>
      </div>
    </el-dialog>
    <selectUnit ref="selectUnit" @handleSetUnit="handleGetUnit"></selectUnit>
  </div>
</template>

<script>
import { postAction, getAction } from "@/api/manage";
import selectUnit from "./components/selectUnit";
export default {
  components: {
    selectUnit,
  },
  data() {
    return {
      dialogVisible: false,
      dialogStatus: "add",
      dataInfo: {
        epName: "",
        unifiedSocialCreditCode: "",
        legalPerson: "",
        linkman: "",
        contactNumber: "",
        address: "",
        password: "",
        title: "",
        content: "",
        now: this.dateFtt('yyyy-MM-dd hh:mm:ss', new Date()),
        status: "",
        whetherUrgent: "",
        message: ''
      },
      rules: {
        epName: [
          {
            required: true,
            trigger: ["change", "blur"],
            message: "请输入企业名称",
          },
        ],
        contactNumber: [
          {
            required: true,
            trigger: ["change", "blur"],
            message: "请输入企业联系人电话",
          },
        ],
        linkman: [
          {
            required: true,
            trigger: ["change", "blur"],
            message: "请输入企业联系人",
          },
        ],
        title: [
          {
            required: true,
            trigger: ["change", "blur"],
            message: "请输入诉求标题",
          },
        ],
        content: [
          {
            required: true,
            trigger: ["change", "blur"],
            message: "请输入诉求内容",
          },
        ],
      },
      unitList: [],
      dataList: [],

      fileHeader: {
        Authorization: "Bearer " + this.$store.state.user.token,
      },
      fileList: [],
      fileListRes: [],
      appealList: [],
      appealListRes: []
    };
  },
  methods: {
    dateFtt(fmt, date) {
      // dateFtt('yyyy-MM-dd hh:mm:ss',a)
      let time;
      if (!date) return "";
      if (typeof date === "string") {
        time = new Date(date.replace(/-/g, "/").replace(/T|Z/g, " ").trim());
      } else if (typeof date === "object") {
        time = new Date(date);
      }
      const o = {
        "M+": time.getMonth() + 1, // 月份
        "d+": time.getDate(), // 日
        "h+": time.getHours(), // 小时
        "m+": time.getMinutes(), // 分
        "s+": time.getSeconds(), // 秒
        "q+": Math.floor((time.getMonth() + 3) / 3), // 季度
        S: time.getMilliseconds(), // 毫秒
      };
      if (/(y+)/.test(fmt)) {
        fmt = fmt.replace(
          RegExp.$1,
          (time.getFullYear() + "").substr(4 - RegExp.$1.length)
        );
      }
      for (var k in o) {
        if (new RegExp("(" + k + ")").test(fmt)) {
          fmt = fmt.replace(
            RegExp.$1,
            RegExp.$1.length == 1
              ? o[k]
              : ("00" + o[k]).substr(("" + o[k]).length)
          );
        }
      }
      return fmt;
    },
    init(status, id) {
      this.dialogVisible = true;
      this.dialogStatus = status;

      this.$nextTick(() => {
        if (status === 'add') {
          this.fileList = []
          this.appealList = []
          this.$refs.dataInfo.resetFields()
        } else {
          this.getDetail(id)
        }
      });
    },
    handleGetFile(file) {
      let resFile = file.data;
      resFile.type = 1;
      resFile.uploadByName = this.$store.state.user.name;
      this.fileListRes.push(resFile);
    },
    handleGetFile2(file) {
      let resFile = file.data;
      resFile.type = 2;
      this.appealListRes.push(resFile);
    },
    handleSelectUnit() {
      this.$refs.selectUnit.init();
    },
    async handleGetUnit(data) {
      const len = data.length;
      if (len > 0) {
        let resArr = [];
        for (let i = 0; i < len; i++) {
          const params = {
            orgId: data[i].id,
          };
          const res = await getAction("/sys/user/queryByOrgId", params);
          if (res.code === 200) {
            resArr.push({
              orgId: data[i].id,
              orgName: data[i].unitName,
              contactNumber: res.data.mobile,
              dealPersonLiable: res.data.userName,
              userId: res.data.userId
            });
          }
        }

        this.unitList = resArr;
      }
    },
    getFlowId() {
      
    },
    handleSubmit() {
      if (this.unitList.length === 0) {
        this.$message({
          type: "warning",
          message: "请选择办理单位！",
        });
        return;
      }
      this.$refs.dataInfo.validate((valid) => {
        if (valid) {
          const params = {
            enterpriseInfo: {
              epName: this.dataInfo.epName,
              unifiedSocialCreditCode: this.dataInfo.unifiedSocialCreditCode,
              legalPerson: this.dataInfo.legalPerson,
              linkman: this.dataInfo.linkman,
              contactNumber: this.dataInfo.contactNumber,
              address: this.dataInfo.address,
              password: this.dataInfo.password,
            },
            appealManage: {
              title: this.dataInfo.title,
              content: this.dataInfo.content,
              whetherUrgent: this.dataInfo.whetherUrgent,
            },
            appealOrgList: this.unitList,
            accessoryInfoList: [...this.fileList, ...this.appealList],
          };

          postAction("/appeal/manage/save", params).then(async (res) => {
            if (res.code === 200) {
              
              this.$message({
                type: "success",
                message: res.msg,
              });
              // this.$emit('handleRefresh')

              getAction('/flowable/processDefinition/list?current=1&size=1&latestVersion=true&key=appeal_key').then(response => {
                if (response.code === 200) {
                  const users = this.unitList.map(el => {
                    return {
                      userId: el.userId,
                      userName: el.dealPersonLiable,
                      orgId: el.orgId
                    }
                  })
                  const flowParams = {
                    processDefinitionId: response.data.records[0].id,
                    values: {
                      apppealOrgs: JSON.stringify(users),
                      message: this.dataInfo.message,
                      urgent: this.dataInfo.whetherUrgent
                    }
                  }

                  postAction('/flowable/processInstance/start', flowParams).then(result => {
                    console.log(result)
                  })
                }
              })
              this.dialogVisible = false;
            } else {
              this.$message({
                type: "warning",
                message: res.msg,
              });
            }
          });
        }
      });
    },
    getDetail(appealId) {
      const params = {
        appealId
      }
      getAction('/appeal/manage/queryById', params).then(res => {
        if (res.code === 200) {
          const { enterpriseInfo, appealManage, appealOrgList, accessoryInfoList } = res.data
          this.dataInfo = {
            epName: enterpriseInfo.epName,
            unifiedSocialCreditCode: enterpriseInfo.unifiedSocialCreditCode,
            legalPerson: enterpriseInfo.legalPerson,
            linkman: enterpriseInfo.linkman,
            contactNumber: enterpriseInfo.contactNumber,
            address: enterpriseInfo.address,
            password: enterpriseInfo.password,
            title: appealManage.title,
            content: appealManage.content,
            now: "",
            status: "",
            whetherUrgent: appealManage.whetherUrgent,
          }

          this.unitList = appealOrgList
          this.fileListRes = accessoryInfoList.filter(el => el.type === 1)
          this.appealListRes = accessoryInfoList.filter(el => el.type === 2)
          
        }
      })
    },
  },
  mounted() {
  },
};
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
  .unit-wrapper {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: red;
    margin-bottom: 10px;
    .unit-item {
      flex: 1;
    }
  }
}
</style>