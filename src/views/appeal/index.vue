<template>
  <div class="app-container">
    <div class="filter-container">
      <el-input
        v-model="listQuery.processInstanceId"
        placeholder="企业名称"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      <el-input
        v-model="listQuery.taskName"
        placeholder="诉求内容"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      <el-input
        v-model="listQuery.processInstanceBusinessKey"
        placeholder="受理交办人"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      <el-date-picker
        v-model="listQuery.dueDateAfter"
        value-format="yyyy-MM-dd"
        placeholder="受理时间（开始）"
        type="date"
        style="width: 200px"
        class="filter-item"
      ></el-date-picker>
      <el-date-picker
        v-model="listQuery.dueDateBefore"
        value-format="yyyy-MM-dd"
        placeholder="受理时间（结束）"
        type="date"
        style="width: 200px"
        class="filter-item"
      ></el-date-picker>
      <el-dropdown
        split-button
        type="primary"
        @click="btnQuery"
        class="filter-item"
      >
        <i class="el-icon-search el-icon--left"></i>查询
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item icon="el-icon-zoom-out" @click.native="btnReset"
            >重置</el-dropdown-item
          >
        </el-dropdown-menu>
      </el-dropdown>
      <el-input
        v-model="listQuery.processInstanceId"
        placeholder="诉求来源"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      <el-input
        v-model="listQuery.taskName"
        placeholder="办理单位"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      <el-input
        v-model="listQuery.processInstanceBusinessKey"
        placeholder="办理责任人"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      <el-select
        v-model="listQuery.sourceStrategy"
        placeholder="紧急程度"
        style="width: 200px"
        class="filter-item"
      >
      </el-select>
      <el-input
        v-model="listQuery.processInstanceBusinessKey"
        placeholder="联系方式"
        style="width: 200px"
        class="filter-item"
        @keyup.enter.native="btnQuery"
      />
      
      <el-button-group>
        <el-button
          type="primary"
          @click="handleAdd('add')"
          class="filter-item"
          >登记诉求</el-button
        >
        <!-- <el-button
          type="primary"
          @click="handleDetail('add')"
          class="filter-item"
          >详情（待删）</el-button
        > -->
      </el-button-group>
    </div>
    <el-table
      :data="records"
      border
      fit
      highlight-current-row
      style="width: 100%"
      :cell-style="{ padding: '3px' }"
    >
      <el-table-column
        label="企业名称"
        prop="processInstanceName"
        align="center"
      >
      </el-table-column>
      <el-table-column label="诉求标题" prop="name" align="center">
      </el-table-column>
      <el-table-column label="受理交办人" prop="name" align="center">
      </el-table-column>
      <el-table-column
        label="受理时间"
        prop="createTime"
        align="center"
        width="165px"
      >
        <template slot-scope="scope"
          ><span>{{ scope.row.createTime }}</span></template
        >
      </el-table-column>
      <el-table-column label="诉求来源" prop="name" align="center">
      </el-table-column>
      <el-table-column label="办理单位" prop="name" align="center">
      </el-table-column>
      <el-table-column label="办理责任人" prop="name" align="center">
      </el-table-column>
      <el-table-column label="紧急程度" prop="name" align="center">
      </el-table-column>
      <el-table-column label="联系方式" prop="name" align="center">
      </el-table-column>
      <el-table-column label="操作" align="center">
        <template slot-scope="{ row }">
          <el-dropdown>
            <span class="el-dropdown-link"
              >操作<i class="el-icon-arrow-down el-icon--right"></i
            ></span>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item
                icon="el-icon-view"
                @click.native="handleAdd('edit', row.processInstanceId)"
                >受理
              </el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </template>
      </el-table-column>
    </el-table>
    <pagination
      v-show="total > 0"
      :total="total"
      :current.sync="listQuery.current"
      :size.sync="listQuery.size"
      @pagination="list"
    />
    <appealDetail ref='appealDetail'></appealDetail>
    <appealForm ref="appealForm"></appealForm>
  </div>
</template>

<script>
import Pagination from "@/components/Pagination";
import { getAction, putAction, postAction, deleteAction } from "@/api/manage";
import { Message } from "element-ui";

import appealDetail from './appealDetail'
import appealForm from './appealForm'

export default {
  name: "appeal",
  components: { Pagination, appealDetail, appealForm },
  data() {
    return {
      dicts: [],
      records: null,
      selectedRecords: [],
      total: 0,
      listQuery: {
        current: 1,
        size: 10,
        processInstanceId: undefined,
        taskName: undefined,
        processInstanceBusinessKey: undefined,
        dueDateAfter: undefined,
        dueDateBefore: undefined,
      },
      dialogExcuteTaskVisible: false,
      formJson: undefined,
      executeTaskId: "",
      processInstanceId: "",
      dialogViewVisible: false,
    };
  },
  beforeCreate() {
    this.getDicts("trueOrFalse").then(({ data }) => {
      this.dicts = data;
    });
  },
  created() {
    this.list();
  },
  methods: {
    handleAdd(status, id = '') {
      this.$refs.appealDetail.init(status, id)
    },
    handleDetail() {
      this.$refs.appealForm.init()
    },
    list() {
      getAction("/flowable/task/listTodo", this.listQuery).then((res) => {
        const { data } = res;
        this.records = data.records;
        this.total = data.total;
      });
    },
    btnQuery() {
      this.listQuery.current = 1;
      this.list();
    },
    btnReset() {
      this.listQuery = {
        current: 1,
        size: 10,
        processInstanceId: undefined,
        taskName: undefined,
        processInstanceBusinessKey: undefined,
        dueDateAfter: undefined,
        dueDateBefore: undefined,
      };
      this.list();
    },
    btnClaim(row) {
      putAction("/flowable/task/claim", { taskId: row.id }).then(({ msg }) => {
        Message.success(msg);
        this.list();
      });
    },
    btnUnclaim(row) {
      putAction("/flowable/task/unclaim", { taskId: row.id }).then(
        ({ msg }) => {
          Message.success(msg);
          this.list();
        }
      );
    },
    btnExcuteTask(row) {
      this.executeTaskId = row.id;
      this.dialogExcuteTaskVisible = true;
    },
    btnView(processInstanceId) {
      this.processInstanceId = processInstanceId;
      this.dialogViewVisible = true;
    },
  },
};
</script>
