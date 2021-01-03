<template>
  <div>
    <el-row>
      <el-col>
        <el-table :data="list" style="width: 100%;margin-top:30px" border>
          <el-table-column align="center" label="序号" width="80">
            <template slot-scope="scope">
              {{ scope.$index+1 }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="主机IP" width="220">
            <template slot-scope="scope">
              {{ scope.row.ip }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="进程号" width="220">
            <template slot-scope="scope">
              {{ scope.row.num }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="进程名" width="220">
            <template slot-scope="scope">
              {{ scope.row.name }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="威胁名称" width="220">
            <template slot-scope="scope">
              {{ scope.row.war }}
            </template>
          </el-table-column>
          <el-table-column align="center" label="威胁详情">
            <template slot-scope="scope">
              <el-button type="primary" size="small" @click="show(scope.row.des)">查看详情</el-button>
            </template>
          </el-table-column>
<!--          <el-table-column align="center" label="查看攻击链">-->
<!--            <el-button type="primary" size="small" @click="atkcenterDialogVisible = true">查看攻击链</el-button>-->
<!--          </el-table-column>-->
        </el-table>
      </el-col>
    </el-row>

    <el-dialog
      title="威胁详情"
      :visible.sync="centerDialogVisible"
      width="60%"
      center
    >
      <span v-text="text" />
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
    <!--    <el-dialog-->
    <!--      :visible.sync="atkcenterDialogVisible"-->
    <!--      width="60%"-->
    <!--      height="80%"-->
    <!--      center-->
    <!--    >-->
    <!--      <span class="home_wrap">-->
    <!--        <div class="pdf_down">-->
    <!--          <div class="pdf_set_left" @click="scaleD()">放大</div>-->
    <!--          <div class="pdf_set_middle" @click="scaleX()">缩小</div>-->
    <!--        </div>-->
    <!--        <div :style="{width:pdf_div_width,margin:'0 auto'}">-->
    <!--          <canvas v-for="page in pdf_pages" :id="'the-canvas'+page" :key="page" />-->
    <!--        </div>-->
    <!--      </span>-->
    <!--    </el-dialog>-->
  </div>
</template>

<script>
export default {
  data() {
    return {
      path: 'ws://topioner.yicp.top/websocket/1',
      text: '',
      list: null,
      // pdf_scale: 1.0, // pdf放大系数
      // pdf_pages: [],
      // pdf_div_width: '',
      // pdf_src: null,
      centerDialogVisible: false
      // atkcenterDialogVisible: false
    }
  },
  created() {
    this.initSocket()
    // this.getList()
  },
  mounted() {
    // this.get_pdfurl()
  },
  destroyed() {
    this.close()
  },
  methods: {
    show(text) {
      this.text = text
      this.centerDialogVisible = true
    },
    initSocket() {
      if (typeof (WebSocket) === 'undefined') {
        alert('您的浏览器不支持socket')
      } else {
        // 实例化socket
        this.socket = new WebSocket(this.path)
        // 监听socket连接
        this.socket.onopen = this.open
        // 监听socket错误信息
        this.socket.onerror = this.error
        // 监听socket消息
        this.socket.onmessage = this.getMessage
        this.socket.onclose = this.close
      }
    },
    open: function() {
      console.log('socket连接成功')
      var old_result1 = sessionStorage.getItem('session_data1')
      var session_result1 = JSON.parse(old_result1)
      this.list = session_result1.data
    },
    error: function() {
      console.log('连接错误')
    },
    getMessage: function(msg) {
      console.log(JSON.parse(msg.data))
      var result = JSON.parse(msg.data)
      this.list = result.data
      sessionStorage.setItem('session_data1', JSON.stringify(result))
      var old_result1 = sessionStorage.getItem('session_data1')
      console.log(JSON.parse(old_result1))
    },
    send: function() {
      this.socket.send()
    },
    close: function() {
      console.log('socket已经关闭')
    }
  }
  // getList() {
  //   var vm = this
  //   this.axios({
  //     method: 'GET',
  //     url: 'https://www.fastmock.site/mock/41d89b2965360f0c6208602bf71e8f16/bb/bb'
  //   }).then(function(resp) {
  //     vm.list = resp.data.data
  //     console.log(resp)
  //   })
  // },
}
</script>
<style lang="scss" scoped>
.home_wrap{
  width: 100%;
  height: 100%;
  .pdf_down{
    position:fixed;
    display: flex;
    z-index: 20;
    right:26px;
    bottom:7%;
    .pdf_set_left{
      width: 30px;
      height: 40px;
      color: #408FFF;
      font-size: 11px;
      padding-top:25px;
      text-align: center;
      margin-right: 5px;
      cursor: pointer;
    }
    .pdf_set_middle{
      width: 30px;
      height: 40px;
      color: #408FFF;
      font-size: 11px;
      padding-top:25px;
      text-align: center;
      margin-right: 5px;
      cursor: pointer;
    }
  }
}
</style>
