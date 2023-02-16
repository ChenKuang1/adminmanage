<template>
  <div>
    <el-button type="primary" @click="dialogFormVisible = true">+新增</el-button>
    <!-- 表格 -->
  <el-card style="width: 100%;margin-top:20px">
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
      type="index"
      width="50">
    </el-table-column>
      <el-table-column
        prop="u_name"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="u_sex"
        label="性别"
        width="180">
        <template slot-scope="s">
          <span v-if="(s.row.u_sex == 1)"><el-tag type="success">男</el-tag></span>
          <span v-if="(s.row.u_sex == 2)"><el-tag type="danger">女</el-tag></span>
        </template>
      </el-table-column>
      <el-table-column
        prop="u_date"
        label="出生日期">
      </el-table-column>
      <el-table-column
        prop="u_phone"
        label="手机号码">
      </el-table-column>
      <el-table-column
        prop="u_email"
        label="Email">
      </el-table-column>
      <el-table-column
        prop="u_address"
        label="地址">
      </el-table-column>
      <el-table-column label="操作">
        <template slot-scope="data">
          <el-button type="primary" size="mini" @click="edit(data.row)">编辑</el-button>
          <el-button type="danger" size="mini" @click="del(data.row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-card>
    <!-- 新增表单 -->
    <el-dialog title="新增用户" :visible.sync="dialogFormVisible" width="50%" :before-close="handleClose">
      <el-form :model="form" label-width="100px" :inline="true" :rules="rules" ref="form">
        <el-form-item label="姓名" :label-width="formLabelWidth" prop="name">
          <el-input placeholder="请输入姓名" v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="性别" :label-width="formLabelWidth" prop="sex">
          <el-radio-group v-model="form.sex">
            <el-radio label="男"></el-radio>
            <el-radio label="女"></el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item label="出生日期" :label-width="formLabelWidth" prop="date">
          <el-col>
            <el-date-picker type="date" placeholder="选择日期" v-model="form.date"></el-date-picker>
          </el-col>
        </el-form-item>
        <el-form-item label="手机号" :label-width="formLabelWidth" prop="phone">
          <el-input placeholder="请输入手机号码" v-model="form.phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Email" :label-width="formLabelWidth" prop="email">
          <el-input placeholder="请输入Email" v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="地址" :label-width="formLabelWidth" prop="address">
          <el-input placeholder="请输入地址" v-model="form.address" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="resetForm()">取 消</el-button>
        <el-button type="primary" @click="submitForm('form')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import { getUser } from '../api'

export default {
  name: 'User',
  data() {
    return {
      dialogTableVisible: false,
      dialogFormVisible: false,
      form: {
        name: '',
        sex: '',
        date: '',
        phone: '',
        email: '',
        address: '',
      },
      formLabelWidth: '120px',
      rules: {  //表单验证
        name: [
          { required: true, message: '请输入姓名', trigger: 'blur' },
        ],
        sex: [
          { required: true, message: '请选择性别', trigger: 'blur' },
        ],
        date: [
          { required: true, message: '请选择出生日期', trigger: 'blur' },
        ],
        phone: [
          { required: true, message: '请输入手机号码', trigger: 'blur' },
        ],
        email: [
          { required: true, message: '请输入Email', trigger: 'blur' },
        ],
        address: [
          { required: true, message: '请输入地址', trigger: 'blur' },
        ],
      },
      tableData: [],
      modalType: 0 //0表示新增 1表示编辑
    }
  },
  created(){
    console.log(this)
    getUser().then((data) => {
      console.log(data,'axixos')
      this.tableData = data.data.userData
    })
  },
  methods: {
    submitForm(form) {
      this.$refs.form.validate((valid) => {
        if (valid) {
          alert('submit!')
          this.dialogFormVisible = false
          this.$refs.form.resetFields()
        } else {
          console.log('error submit!!')
          return false;
        }
      })
    },
    handleClose() {
      this.dialogFormVisible = false
      this.$refs.form.resetFields()
    },
    resetForm() {
      this.handleClose
    }
  }
}

</script>

<style scoped>

</style>
