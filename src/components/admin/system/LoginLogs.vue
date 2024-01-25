<template>
  <div class="app-container">
    <el-row style="margin: 18px 1px 1px 18px ">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/admin/dashboard' }">管理中心</el-breadcrumb-item>
        <el-breadcrumb-item>系统管理</el-breadcrumb-item>
        <el-breadcrumb-item>登录日志</el-breadcrumb-item>
      </el-breadcrumb>
    </el-row>
    <el-form label-width="80px" inline>
      <el-form-item label="登录用户">
        <el-input v-model="username"></el-input>
      </el-form-item>


      <el-form-item label="登录时间">
        <el-date-picker
            v-model="startTime"
            type="date"
            placeholder="开始日期"
            format="yyyy-MM-dd HH:mm:ss"
            @change="handleTimeChange"
        />
        <el-date-picker
            v-model="endTime"
            type="date"
            placeholder="结束日期"
            format="yyyy-MM-dd HH:mm:ss"
            @change="handleTimeChange"
        />
      </el-form-item>


      <el-form-item>
        <el-button type="primary" @click="fetchData">搜索</el-button>
        <el-button type="primary" @click="reset">重置</el-button>
      </el-form-item>
    </el-form>


    <el-table :data="loginLogsList" border fit highlight-current-row>
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index + 1 }}
        </template>
      </el-table-column>

      <el-table-column label="登录用户" align="center">
        <template slot-scope="scope">
          {{ scope.row.username }}
        </template>
      </el-table-column>

      <el-table-column label="登录IP地址" align=" center">
        <template slot-scope="scope">
          {{ scope.row.ipAddress }}
        </template>
      </el-table-column>

      <el-table-column label="状态" align="center">
        <template slot-scope="scope">
          {{ scope.row.status == 1 ? "成功" : "失败" }}
        </template>
      </el-table-column>

      <el-table-column label="登录信息" align="center">
        <template slot-scope="scope">
          {{ scope.row.message }}
        </template>
      </el-table-column>

      <el-table-column label="登录时间" align="center">
        <template slot-scope="scope">
          {{ scope.row.loginTime }}
        </template>
      </el-table-column>
    </el-table>


    <div class="block" style="text-align: center;margin-top: 20px;">
      <el-pagination @current-change="fetchData" :current-page.sync="currentPage" :page-size="size"
                     layout="prev, pager, next, jumper" :total="total">
      </el-pagination>
    </div>
  </div>
</template>

<script>


export default {
  name: 'LoginLogs',
  data() {
    return {
      currentPage: 1,
      size: 5,
      total: 100,
      username: null,
      loginLogsList: [],
      startTime: null,
      endTime: null

    }
  },
  mounted() {
    this.fetchData();
  },

  methods: {
    fetchData() {
      this.$axios
          .get('/login_logs/get', {
            params: {
              username: this.username,
              startTime: this.startTime,
              endTime: this.endTime,
              currentPage: this.currentPage,
              size: this.size,
            }
          })
          .then(resp => {
            this.loginLogsList = resp.data.result.records;
            this.total = resp.data.result.total;
          })
          .catch(error => {
            console.error(error);
          });
    },
    reset() {
      this.username = null;
      this.startTime = null;
      this.endTime = null;
      this.fetchData();
    }
  }
}
</script>

<style scoped>
.add-link {
  margin: 18px 0 15px 10px;
  float: left;
}
</style>
