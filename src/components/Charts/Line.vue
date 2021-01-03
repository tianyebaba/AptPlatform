<template>
  <div :id="id" :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
// import resize from './mixins/resize'

export default {
  // mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'linepic'
    },
    id: {
      type: String,
      default: 'linepic'
    },
    width: {
      type: String,
      default: '1150px'
    },
    height: {
      type: String,
      default: '500px'
    }
  },
  data() {
    return {
      path: 'ws://topioner.yicp.top/websocketTwo/1',
      chart: null,
      list: []
    }
  },
  mounted() {
    this.initSocket()
    // this.getList()
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart(list) {
      this.chart = echarts.init(document.getElementById(this.id))

      this.chart.setOption({
        title: {
          text: '数据压缩比实时图'
        },
        tooltip: {
          trigger: 'axis'
        },
        legend: {
          data: ['File read压缩比', 'File write压缩比', 'Process压缩比', 'Network压缩比', 'Image压缩比', 'File_delete_rename压缩比']
        },
        grid: {
          left: '3%',
          right: '4%',
          bottom: '3%',
          containLabel: true
        },
        toolbox: {
          feature: {
            saveAsImage: {}
          }
        },
        xAxis: {
          type: 'category',
          boundaryGap: false,
          data: list[0].date
        },
        yAxis: {
          name: '压缩前后比例（%）',
          type: 'value',
          max: 20
        },
        series: [
          {
            name: 'File read压缩比',
            type: 'line',
            data: list[0].num
          },
          {
            name: 'File write压缩比',
            type: 'line',
            data: list[1].num
          },
          {
            name: 'Process压缩比',
            type: 'line',
            data: list[2].num
          },
          {
            name: 'Network压缩比',
            type: 'line',
            data: list[3].num
          },
          {
            name: 'Image压缩比',
            type: 'line',
            data: list[4].num
          },
          {
            name: 'File_delete_rename压缩比',
            type: 'line',
            data: list[5].num
          }
        ]
      })
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
      var old_result2 = sessionStorage.getItem('session_data2')
      var session_result2 = JSON.parse(old_result2)
      this.list = session_result2.data3
      this.initChart(this.list)
    },
    error: function() {
      console.log('连接错误')
    },
    getMessage: function(msg) {
      console.log(JSON.parse(msg.data))
      var result = JSON.parse(msg.data)
      this.list = result.data3
      sessionStorage.setItem('session_data2', JSON.stringify(result))
      this.initChart(this.list)
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
    //     vm.list = resp.data.data3
    //     vm.initChart(vm.list)
    //   })
    // }
  }
}
</script>
