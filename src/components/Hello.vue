
<template>
  <div>
    <el-button type="text" @click="dialogFormVisible = true">打开嵌套表单的 Dialog</el-button>
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
    <el-dialog title="信息" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="名字" :label-width="formLabelWidth">
          <el-input v-model="rowtemplate.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="地址" :label-width="formLabelWidth">
          <el-select v-model="rowtemplate.address" placeholder="请选择地址">
            <el-option label="上海市普陀区金沙江路 6666 弄" value="上海市普陀区金沙江路 6666 弄"></el-option>
            <el-option label="上海市普陀区金沙江路 2333 弄" value="上海市普陀区金沙江路 2333 弄"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item label="日期" :label-width="formLabelWidth">
          <span class="demonstration">默认</span>
            <el-date-picker
              v-model="rowtemplate.date"
              type="date"
              placeholder="选择日期"
              value-format="yyyy-MM-dd">
            </el-date-picker>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="cancleEdit()">取 消</el-button>
        <el-button type="primary" @click="handleEdit()">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        tableData: [{
          date: '2016-05-02',
          name: '王小虎1',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-04',
          name: '王小虎2',
          address: '上海市普陀区金沙江路 1517 弄'
        }, {
          date: '2016-05-01',
          name: '王小虎3',
          address: '上海市普陀区金沙江路 1519 弄'
        }, {
          date: '2016-05-03',
          name: '王小虎4',
          address: '上海市普陀区金沙江路 1516 弄'
        }],
        rowtemplate: {date: '', name: '', address: ''},
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
      handleEdit(){
        this.tableData.push(this.rowtemplate)
        this.rowtemplate={date: '', name: '', address: ''}
        this.dialogFormVisible = false
      },
      cancleEdit(){
        this.dialogFormVisible = false
        this.rowtemplate={date: '', name: '', address: ''}
      }
    },
  }
</script>
