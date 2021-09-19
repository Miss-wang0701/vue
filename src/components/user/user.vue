<template>
  <el-card class="box-card">
    <!-- 面包屑 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item>首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索 -->
    <el-row class="searchRow">
      <el-col>
        <el-input clearable placeholder="请输入内容" v-model="query" class="input-with-select">
          <el-button @click='getUserList()' slot="append" icon="el-icon-search"></el-button>
        </el-input>
        <el-button type="primary">添加</el-button>
      </el-col>
    </el-row>
    <!-- 表格 -->
    <el-table
      :data="tableData"
      style="width: 100%">
      <el-table-column
        type="index"
        prop="date"
        label="#"
        width="180">
      </el-table-column>
      <el-table-column
        prop="username"
        label="姓名"
        width="180">
      </el-table-column>
      <el-table-column
        prop="email"
        label="邮箱">
      </el-table-column>
      <el-table-column
        prop="mobile"
        label="电话">
      </el-table-column>
      <el-table-column
        prop="create_time"
        label="创建时间">
            <template slot-scope="tableData">
                {{tableData.row.create_time | fmtdate}}
            </template>
      </el-table-column>
      <el-table-column
        prop="mg_state"
        label="用户状态">
            <template slot-scope="scope">
                <el-switch
                    v-model="scope.row.mg_state"
                    active-color="#13ce66"
                    inactive-color="#ff4949">
                </el-switch>
            </template>
      </el-table-column>
      <el-table-column
        prop="address"
        label="操作">
        <template  slot-scope="scope">
            <el-button size='mini' plain type="primary" icon="el-icon-edit" circle></el-button>
            <el-button size='mini' plain type="success" icon="el-icon-check" circle></el-button>
            <el-button size='mini' plain type="danger" icon="el-icon-delete" circle></el-button>
        </template>
      </el-table-column>
    </el-table>
    <!-- 分页 -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="pagenum"
      :page-sizes="[1, 2, 3, 4]"
      :page-size="pagesize"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>

    <!-- 对话框 -->
    <el-dialog title="收货地址" :visible.sync="dialogFormVisible">
        <el-form :model="form">
            <el-form-item label="活动名称" :label-width="formLabelWidth">
            <el-input v-model="form.name" autocomplete="off"></el-input>
            </el-form-item>
            <el-form-item label="活动区域" :label-width="formLabelWidth">
            <el-select v-model="form.region" placeholder="请选择活动区域">
                <el-option label="区域一" value="shanghai"></el-option>
                <el-option label="区域二" value="beijing"></el-option>
            </el-select>
            </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
            <el-button @click="dialogFormVisible = false">取 消</el-button>
            <el-button type="primary" @click="dialogFormVisible = false">确 定</el-button>
        </div>
    </el-dialog>
  </el-card>
</template>

<script>
export default {
  name: "",
  data() {
    return {
      query: "",
      tableData:[],
      pagenum:1,
      pagesize:1,
      total:0
    };
  },
  created () {
      this.getUserList()
  },
  methods: {
    // 获得用户列表
    async getUserList () {
        const token = localStorage.getItem('token')
        this.$http.defaults.headers.common['Authorization'] = token
        const res = await this.$http.get(
            `users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`
        );
        const {meta: {status,msg}, data: {users,total}} = res.data
        if( status === 200 ){
            this.tableData = users;
            this.total = total;
            this.$message.success('操作成功')
        }else{
            this.$message.error('操作失败')
        }
        
    },
    handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
        this.pagesize = val;
        this.pagenum = 1;
        this.getUserList()
    },
    handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
        this.pagenum = val;
        this.getUserList()
    },

  }
};
</script>

<style lang="less" scoped>
.box-card {
  height: 100%;
}
.input-with-select{
    width: 300px;
}
.searchRow{
    margin-top: 20px;
}
</style>