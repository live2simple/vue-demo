<template>
    <div class="app-container">
        <div class="filter-container">
            <el-input v-model="listQuery.title" placeholder="诉求标题" style="width: 200px;" class="filter-item" @keyup.enter.native="btnQuery"/>
            <el-select v-model="listQuery.status" placeholder="诉求状态" class="filter-item"><el-option v-for="(item, index) in dicts.status" :key="index" :label="item.content" :value="item.value"></el-option></el-select>
            <el-select v-model="listQuery.securityClassification" placeholder="密级" class="filter-item"><el-option v-for="(item, index) in dicts.security_classification" :key="index" :label="item.content" :value="item.value"></el-option></el-select>
            <el-select v-model="listQuery.whetherUrgent" placeholder="紧急程度" class="filter-item"><el-option v-for="(item, index) in dicts.whether_urgent" :key="index" :label="item.content" :value="item.value"></el-option></el-select>
            <el-dropdown split-button type="primary" @click="btnQuery" class="filter-item">
                <i class="el-icon-search el-icon--left"></i>查询
                <el-dropdown-menu slot="dropdown">
                    <el-dropdown-item icon="el-icon-zoom-out" @click.native="btnReset">重置</el-dropdown-item>
                </el-dropdown-menu>
            </el-dropdown>
            <el-button-group>
                <el-button v-permission="'appeal:manage:save'" icon="el-icon-plus" type="primary" @click="btnCreate" class="filter-item">新增</el-button>
                <el-button v-permission="'appeal:manage:delete'" icon="el-icon-delete" @click="btnDelete()" class="filter-item">批量删除</el-button>
            </el-button-group>
        </div>
        <el-table
                :data="records"
                ref="multipleTable"
                @selection-change="selectionChange"
                border
                fit
                highlight-current-row
                style="width: 100%;"
                :cell-style="{padding:'3px'}"
        >
            <el-table-column type="selection" align="center">
            </el-table-column>
            <el-table-column label="诉求标题" prop="title" align="center"><template slot-scope="scope"><span>{{ scope.row.title }}</span></template></el-table-column>
            <el-table-column label="诉求状态" prop="status" align="center"><template slot-scope="scope"><span v-html="formatDictText(dicts.status,scope.row.status)"></span></template></el-table-column>
            <el-table-column label="密级" prop="securityClassification" align="center"><template slot-scope="scope"><span v-html="formatDictText(dicts.security_classification,scope.row.securityClassification)"></span></template></el-table-column>
            <el-table-column label="紧急程度" prop="whetherUrgent" align="center"><template slot-scope="scope"><span v-html="formatDictText(dicts.whether_urgent,scope.row.whetherUrgent)"></span></template></el-table-column>
            <el-table-column label="操作" align="center">
                <template slot-scope="{row}">
                    <el-dropdown>
                        <span class="el-dropdown-link">操作<i class="el-icon-arrow-down el-icon--right"></i></span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item icon="el-icon-view" @click.native="btnView(row)">查看</el-dropdown-item>
                            <el-dropdown-item v-permission="'appeal:manage:update'" icon="el-icon-edit" divided @click.native="btnUpdate(row)">修改</el-dropdown-item>
                            <el-dropdown-item v-permission="'appeal:manage:delete'" icon="el-icon-delete" divided @click.native="btnDelete(row.id)">删除</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>
                </template>
            </el-table-column>
        </el-table>
        <pagination v-show="total>0" :total="total" :current.sync="listQuery.current" :size.sync="listQuery.size"
                    @pagination="list"/>

        <el-dialog title="" :visible.sync="dialogFormVisible">
            <el-form ref="dataForm" :rules="rules" :model="temp" :disabled="dialogStatus==='view'" label-position="right" label-width="110px">
                <el-form-item label="主键id" prop="id"><el-input v-model="temp.id" :readonly="dialogStatus==='update'"/></el-form-item>
                <el-form-item label="诉求标题" prop="title"><el-input v-model="temp.title"/></el-form-item>
                <el-form-item label="诉求内容" prop="content"><el-input v-model="temp.content"/></el-form-item>
                <!--<el-form-item label="诉求状态" prop="status"><el-select v-model="temp.status" placeholder="诉求状态"><el-option v-for="(item, index) in dicts.status" :key="index" :label="item.content" :value="item.value"></el-option></el-select></el-form-item>-->
                <!--<el-form-item label="来文流水号" prop="receivingDocumentSerialNumber"><el-input v-model="temp.receivingDocumentSerialNumber"/></el-form-item>-->
                <!--<el-form-item label="密级" prop="securityClassification"><el-select v-model="temp.securityClassification" placeholder="密级"><el-option v-for="(item, index) in dicts.security_classification" :key="index" :label="item.content" :value="item.value"></el-option></el-select></el-form-item>-->
                <!--<el-form-item label="紧急程度" prop="whetherUrgent"><el-select v-model="temp.whetherUrgent" placeholder="紧急程度"><el-option v-for="(item, index) in dicts.whether_urgent" :key="index" :label="item.content" :value="item.value"></el-option></el-select></el-form-item>-->
                <!--<el-form-item label="受理交办人" prop="accepter"><el-input v-model="temp.accepter"/></el-form-item>-->
                <!--<el-form-item label="受理交办人id" prop="accepterId"><el-input v-model="temp.accepterId"/></el-form-item>-->
                <!--<el-form-item label="受理时间" prop="acceptTime"><el-date-picker v-model="temp.acceptTime" type="datetime" value-format="yyyy-MM-dd HH:mm:ss"></el-date-picker></el-form-item>-->
                <!--<el-form-item label="解决措施" prop="solution"><el-input v-model="temp.solution"/></el-form-item>-->
                <!--<el-form-item label="回复企业" prop="reply"><el-input v-model="temp.reply"/></el-form-item>-->
                <!--<el-form-item label="评价-综合评分" prop="evaluateScore"><el-input v-model="temp.evaluateScore"/></el-form-item>-->
                <!--<el-form-item label="评价内容" prop="evaluateContent"><el-input v-model="temp.evaluateContent"/></el-form-item>-->
                <el-form-item label="外键关联企业主键id" prop="epId"><el-input v-model="temp.epId"/></el-form-item>
                <!--<el-form-item label="逻辑删除标志" prop="whetherDel"><el-input v-model="temp.whetherDel"/></el-form-item>-->
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button icon="el-icon-close" @click="dialogFormVisible = false">取消</el-button>
                <el-button v-if="dialogStatus!=='view'" icon="el-icon-check" type="primary" @click="dialogStatus==='create'?createData():updateData()">确定</el-button>
            </div>
        </el-dialog>
    </div>
</template>

<script>
    import Pagination from '@/components/Pagination'
    import {getAction, putAction, postAction, deleteAction} from '@/api/manage'
    import {Message} from 'element-ui'

    export default {
        name: 'Appeal',
        components: {Pagination},
        data() {
            return {
                dicts: [],
                records: null,
                selectedRecords: [],
                total: 0,
                listQuery: {
                    current: 1,
                    size: 10,
                    title: undefined,
                    status: undefined,
                    securityClassification: undefined,
                    whetherUrgent: undefined
                },
                dialogFormVisible: false,
                dialogStatus: '',
                temp: {
                    id: undefined,
                    title: '',
                    content: '',
                    status: '',
                    receivingDocumentSerialNumber: '',
                    securityClassification: '',
                    whetherUrgent: '',
                    accepter: '',
                    accepterId: '',
                    acceptTime: '',
                    solution: '',
                    reply: '',
                    evaluateScore: '',
                    evaluateContent: '',
                    epId: '',
                    whetherDel: ''
                },
                rules: {
                    id: [{required: true, message: '该项不能为空', trigger: 'change'}],
                    title: [{required: true, message: '该项不能为空', trigger: 'change'}],
                    content: [{required: true, message: '该项不能为空', trigger: 'change'}],
                    status: [{required: true, message: '该项不能为空', trigger: 'change'}]
                }
            }
        },
        beforeCreate(){
            this.getDicts('status,security_classification,whether_urgent').then(({data}) => {this.dicts = data})
        },
        created() {
            this.list()
        },
        methods: {
            list() {
                getAction('/appeal/manage/list', this.listQuery).then(res => {
                    const {data} = res
                    this.records = data.records;
                    this.total = data.total
                })
            },
            btnQuery() {
                this.listQuery.current = 1
                this.list()
            },
            btnReset() {
                this.listQuery = {
                    current: 1,
                    size: 10,
                    title: undefined,
                    status: undefined,
                    securityClassification: undefined,
                    whetherUrgent: undefined
                }
                this.list()
            },
            resetTemp() {
                this.temp = {
                    id: undefined,
                    title: '',
                    content: '',
                    status: '',
                    receivingDocumentSerialNumber: '',
                    securityClassification: '',
                    whetherUrgent: '',
                    accepter: '',
                    accepterId: '',
                    acceptTime: '',
                    solution: '',
                    reply: '',
                    evaluateScore: '',
                    evaluateContent: '',
                    epId: '',
                    whetherDel: ''
                }
            },
            btnView(row) {
                this.temp = Object.assign({}, row)
                this.dialogStatus = 'view'
                this.dialogFormVisible = true
                this.$nextTick(() => {
                    this.$refs['dataForm'].clearValidate()
                })
            },
            btnCreate() {
                this.resetTemp()
                this.dialogStatus = 'create'
                this.dialogFormVisible = true
                this.$nextTick(() => {
                    this.$refs['dataForm'].clearValidate()
                })
            },
            createData() {
                this.$refs['dataForm'].validate((valid) => {
                    if (valid) {
                        postAction('/appeal/manage/save', this.temp).then(({msg}) => {
                            this.dialogFormVisible = false
                            Message.success(msg)
                            this.list()
                        })
                    }
                })
            },
            btnUpdate(row) {
                this.temp = Object.assign({}, row)
                this.dialogStatus = 'update'
                this.dialogFormVisible = true
                this.$nextTick(() => {
                    this.$refs['dataForm'].clearValidate()
                })
            },
            updateData() {
                this.$refs['dataForm'].validate((valid) => {
                    if (valid) {
                        putAction('/appeal/manage/update', this.temp).then(({msg}) => {
                            this.dialogFormVisible = false
                            Message.success(msg)
                            this.list()
                        })
                    }
                })
            },
            btnDelete(id) {
                let ids = id ? [id] : this.selectedRecords.map(record => {
                    return record.id
                })
                if (ids.length == 0) {
                    Message.error('请选择要删除的记录')
                    return
                }
                deleteAction('/appeal/manage/delete', {ids: ids.toString()}).then(({msg}) => {
                    Message.success(msg)
                    this.list()
                })
            },
            selectionChange(selectedRecords) {
                this.selectedRecords = selectedRecords
            }
        }
    }
</script>
