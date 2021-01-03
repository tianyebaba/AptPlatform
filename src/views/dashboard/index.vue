<template>
  <div class="board-container bg-purple-light">
    <el-row :gutter="1">
      <el-col :span="4">
        <div class="grid-content2 row-bg">
          <div class="computer-pic" align="center">
            <i class="el-icon-s-platform"></i>
            <div class="computer-info" align="center">
              <el-row>
                <el-col>主机IP：127.0.1</el-col>
              </el-row>
            </div>
          </div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content2 row-bg">
          <div class="computer-pic" align="center">
            <i class="el-icon-s-platform"></i>
            <div class="computer-info" align="center">
              <el-row>
                <el-col>主机IP：127.0.2</el-col>
              </el-row>
            </div>
          </div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content2 row-bg">
          <div class="computer-pic" align="center">
            <i class="el-icon-s-platform"></i>
            <div class="computer-info" align="center">
              <el-row>
                <el-col>主机IP：127.0.3</el-col>
              </el-row>
            </div>
          </div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content2 row-bg">
          <div class="computer-pic" align="center">
            <i class="el-icon-s-platform"></i>
            <div class="computer-info" align="center">
              <el-row>
                <el-col>主机IP：127.0.4</el-col>
              </el-row>
            </div>
          </div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content2 row-bg">
          <div class="computer-pic" align="center">
            <i class="el-icon-s-platform"></i>
            <div class="computer-info" align="center">
              <el-row>
                <el-col>主机IP：127.0.5</el-col>
              </el-row>
            </div>
          </div>
        </div>
      </el-col>
      <el-col :span="4">
        <div class="grid-content2 row-bg">
          <div class="computer-pic" align="center">
            <i class="el-icon-s-platform"></i>
            <div class="computer-info" align="center">
              <el-row>
                <el-col>主机IP：127.0.6</el-col>
              </el-row>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>

export default {
  data() {
    return {
      centerDialogVisible: false,
      tableData: [{
        date: '2020-09-05 11:22:34',
        ip: '127.0.1',
        name: 'xxx'
      }, {
        date: '2020-09-05 11:22:35',
        ip: '127.0.2',
        name: 'xxxx'
      }, {
        date: '2020-09-05 11:22:36',
        ip: '127.0.3',
        name: 'xxxxx'
      }]
    }
  }
}
</script>

<style>
.computer-pic{
  font-size: 130px;
}
.computer-info{
  font-size: 15px;
}
.bg-purple {
  background: #d3dce6;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content2 {
  margin-top: 30px;
  margin-left: 30px;
  margin-right: 30px;
  margin-bottom: 30px;
  padding: 10px 15px 15px 10px;
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px;
  background-color: #f9fafc;
}
</style>
