<template>
  <div class="dashboard-container bg-purple-light">
    <el-row>
      <el-col :span="12"><div class="grid-content3 bg-purple-light">
        <div class="grid-content4 row-bg">
          <Graph height="100%" width="100%" />
        </div>
      </div></el-col>
      <el-col :span="12"><div class="grid-content3 bg-purple-light">
        <div class="grid-content4 row-bg">
          <el-table
            :data=null
            style="width: 100%">
            <el-table-column
              prop="date"
              label="日期"
              width="180">
            </el-table-column>
            <el-table-column
              prop="name"
              label="姓名"
              width="180">
            </el-table-column>
            <el-table-column
              prop="address"
              label="地址">
            </el-table-column>
          </el-table>
        </div>
      </div></el-col>
    </el-row>
  </div>
</template>

<script>
import Graph from '@/components/Charts/Graph'
export default {
  components: { Graph }
}
</script>

<style>
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content3 {
  margin-right: 20px;
  border-radius: 4px;
  min-height: 740px;
}
.row-bg {
  padding: 10px;
  background-color: #f9fafc;
}
.grid-content4{
  position: relative;
  width: 100%;
  height: calc(100vh - 84px);
  margin-top: 20px;
  margin-right: 20px;
  margin-left: 20px;
  border-radius: 4px;
  min-height: 700px;
}
</style>
