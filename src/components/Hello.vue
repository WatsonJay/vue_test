
<template>
  <div style="width: 90%;margin-left: auto;margin-right: auto;">
    <el-button type="text" @click="dialogFormVisible = true">新增信息</el-button>
    <el-table
      :data="tableData.filter(data => !search || data.address.toLowerCase().includes(search.toLowerCase()))"
      style="width: 100%">
      <el-table-column
        label="Date"
        prop="date">
      </el-table-column>
      <el-table-column
        label="Name"
        prop="name">
      </el-table-column>
      <el-table-column
        prop="address"
        label="地址">
      </el-table-column>
      <el-table-column
        align="right">
        <template slot="header" slot-scope="scope">
          <el-input
            v-model="search"
            size="mini"
            placeholder="输入关键字搜索"/>
        </template>
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="openDialog(scope.row)">Edit</el-button>
          <el-button
            size="mini"
            type="danger"
            @click.native.prevent="handleDelete(scope.$index, tableData)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog title="信息" :visible.sync="dialogFormVisible" :before-close="handleClose" style="width: 60%;margin-left: auto;margin-right: auto;">
      <div style="width: 90%;margin-left: auto;margin-right: auto;">
        <el-form :model="rowtemplate" :rules="rules" ref="infoform">
          <el-form-item label="名字:" :label-width="formLabelWidth"  prop="name">
            <el-input size="medium" v-model="rowtemplate.name" autocomplete="off" style="width: 240px"></el-input>
          </el-form-item>
          <el-form-item label="地址:" :label-width="formLabelWidth"  prop="address">
            <el-select v-model="rowtemplate.address" placeholder="请选择地址" style="width: 240px">
              <el-option label="上海市普陀区金沙江路 6666 弄" value="上海市普陀区金沙江路 6666 弄"></el-option>
              <el-option label="上海市普陀区金沙江路 2333 弄" value="上海市普陀区金沙江路 2333 弄"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="日期:" :label-width="formLabelWidth" prop="date" >
            <el-date-picker
              v-model="rowtemplate.date"
              type="date"
              placeholder="选择日期"
              value-format="yyyy-MM-dd"
              style="width: 240px">
            </el-date-picker>
          </el-form-item>
        </el-form>
      </div>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancleEdit()">取 消</el-button>
        <el-button type="primary" @click="handleEdit('infoform')">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        tableData: [{
          Id: 1,
          date: '2016-05-02',
          name: '王小虎1',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          Id: 2,
          date: '2016-05-04',
          name: '王小虎2',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          Id: 3,
          date: '2016-05-01',
          name: '王小虎3',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          Id: 4,
          date: '2016-05-03',
          name: '王小虎4',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        rowtemplate: {Id: 0, date: '', name: '', address: ''},
        rules:{
          name:[
            { required: true, message: '请输入姓名', trigger: 'blur' },
            { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
          ],
          address:[
            { required: true, message: '请输入地址', trigger: 'change' }
          ],
          date:[
            { required: true, message: '请选择日期', trigger: 'change' }
          ],
        },
        dialogFormVisible: false,
        search: '',
        formLabelWidth: '120px'
      }
    },
    methods: {
      openDialog(row) {
        this.dialogFormVisible = true
        this.rowtemplate = row
      },
      handleDelete(index, rows) {
        rows.splice(index, 1);
      },
      handleEdit(formName){
        this.$refs[formName].validate((valid) => {
          if (valid) {
            if (this.rowtemplate.Id == 0) {
              //设置当前新增行的Id
              this.rowtemplate.Id = this.tableData.length + 1;
              this.tableData.push(this.rowtemplate)
            }
            this.rowtemplate = {Id: 0, date: '', name: '', address: ''}
            this.dialogFormVisible = false
          } else {
            return false;
          }
        });
      },
      cancleEdit(){
        this.dialogFormVisible = false
        this.rowtemplate={Id: 0, date: '', name: '', address: ''}
      },
      handleClose(done){
        this.rowtemplate={Id: 0, date: '', name: '', address: ''}
        done();
      }
    },
  }
</script>
