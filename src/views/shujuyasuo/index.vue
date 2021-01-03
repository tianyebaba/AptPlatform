<template>
  <div>
    <el-row>
      <el-col :span="24"><div class="grid-content1 bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
      <el-col :span="1"><div class="grid-content bg-purple-light"></div></el-col>
      <el-col :span="10">
        <div class="grid-content row-bg">
          <chart-a />
        </div>
      </el-col>
      <el-col :span="12">
        <div class="grid-content row-bg">
          <el-table
            ref="filterTable"
            :data="list1"
            style="width: 100%">
            <el-table-column align="center" label="序号" width="50">
              <template slot-scope="scope">
                {{ scope.$index+1 }}
              </template>
            </el-table-column>
            <el-table-column
              align="center"
              prop="num"
              label="数量"
              width="160">
            </el-table-column>
            <el-table-column
              prop="tag"
              label="类型"
              width="140">
              <template slot-scope="scope">
                <el-tag :type="scope.row.tag | statusFilter">{{ scope.row.tag }}</el-tag>
              </template>
            </el-table-column>
            <el-table-column
              align="center"
              prop="date"
              label="时间戳"
              sortable
              column-key="date"
            >
            </el-table-column>
          </el-table>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
      <el-col :span="24"><div class="grid-content1 bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
      <el-col :span="1"><div class="grid-content bg-purple-light"></div></el-col>
      <el-col :span="10">
        <div class="grid-content row-bg">
          <chart-b />
        </div>
      </el-col>
      <el-col :span="12">
        <div class="grid-content row-bg">
          <el-table
            ref="filterTable"
            :data="list2"
            style="width: 100%">
            <el-table-column align="center" label="序号" width="50">
              <template slot-scope="scope">
                {{ scope.$index+1 }}
              </template>
            </el-table-column>
            <el-table-column
              align="center"
              prop="num"
              label="数量"
              width="160">
            </el-table-column>
            <el-table-column
              prop="tag"
              label="类型"
              width="140">
              <template slot-scope="scope">
                <el-tag :type="scope.row.tag | statusFilter">{{ scope.row.tag }}</el-tag>
              </template>
            </el-table-column>
            <el-table-column
              align="center"
              prop="date"
              label="时间戳"
              sortable
              column-key="date"
            >
            </el-table-column>
          </el-table>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
      <el-col :span="24"><div class="grid-content1 bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
      <el-col :span="1"><div class="grid-content bg-purple-light"></div></el-col>
      <el-col :span="22">
        <div>
          <line-pic/>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple-light"></div></el-col>
    </el-row>
    <el-row>
      <el-col :span="24"><div class="grid-content1 bg-purple-light"></div></el-col>
    </el-row>
  </div>
</template>

<script>
import ChartA from '@/components/Charts/Pie'
import ChartB from '@/components/Charts/Pie1'
import LinePic from '@/components/Charts/Line'
export default {
  components: { ChartA, ChartB, LinePic },
  filters: {
    statusFilter(status) {
      const statusMap = {
        File_read: 'success',
        File_write: 'gray',
        Process: 'danger',
        Network: 'warning',
        Image: 'info',
        File_delete_rename: 'gray',
        Amount: 'gray'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      path: 'ws://topioner.yicp.top/websocketTwo/1',
      list1: null,
      list2: null
    }
  },
  created() {
    this.initSocket()
    // this.getList()
  },
  destroyed() {
    this.close()
  },
  methods: {
    formatter(row, column) {
      return row.address
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
      var old_result5 = sessionStorage.getItem('session_data5')
      var session_result5 = JSON.parse(old_result5)
      this.list1 = session_result5.data1
      this.list2 = session_result5.data2
    },
    error: function() {
      console.log('连接错误')
    },
    getMessage: function(msg) {
      var result = JSON.parse(msg.data)
      sessionStorage.setItem('session_data5', JSON.stringify(result))
      var old_result5 = sessionStorage.getItem('session_data5')
      console.log(JSON.parse(old_result5))
      this.list1 = result.data1
      this.list2 = result.data2
    },
    send: function() {
      this.socket.send()
    },
    close: function() {
      console.log('socket已经关闭')
    }
    // getList() {
    //   var vm = this
    //   this.axios({
    //     method: 'GET',
    //     url: 'https://www.fastmock.site/mock/abae797a4d0955b9ac601e85b84dca02/aa/aa'
    //   }).then(function(resp) {
    //     vm.list1 = resp.data.data1
    //     console.log(resp.data)
    //     console.log(resp.data.data1)
    //     vm.list2 = resp.data.data2
    //   })
    // }
  }
}
</script>

<style>
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content1 {
  border-radius: 0px;
  min-height: 18px;
}
.grid-content {
  border-radius: 0px;
  min-height: 500px;
}
.row-bg {
  padding: 10px;
  background-color: #f9fafc;
}
</style>
