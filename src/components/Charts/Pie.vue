<template>
  <div :id="id" :class="className" :style="{height:height,width:width}" />
</template>

<script>
import echarts from 'echarts'
// import resize from './mixins/resize'
export default {
  name: 'ChartA',
  // mixins: [resize],
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    id: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '500px'
    },
    height: {
      type: String,
      default: '480px'
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
          text: '压缩前',
          left: 'center'
        },
        tooltip: {
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        legend: {
          orient: 'vertical',
          left: 'left',
          data: ['File read', 'File write', 'Process/Thread', 'Network', 'Image', 'File_delete_rename']
        },
        series: [
          {
            name: '来源',
            type: 'pie',
            radius: '50%',
            center: ['55%', '58%'],
            data: [
              { value: list[1].num, name: 'File read' },
              { value: list[2].num, name: 'File write' },
              { value: list[3].num, name: 'Process/Thread' },
              { value: list[4].num, name: 'Network' },
              { value: list[5].num, name: 'Image' },
              { value: list[6].num, name: 'File_delete_rename' }
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
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
      var old_result3 = sessionStorage.getItem('session_data3')
      var session_result3 = JSON.parse(old_result3)
      this.list = session_result3.data1
      this.initChart(this.list)
    },
    error: function() {
      console.log('连接错误')
    },
    getMessage: function(msg) {
      console.log(JSON.parse(msg.data))
      var result = JSON.parse(msg.data)
      this.list = result.data1
      sessionStorage.setItem('session_data3', JSON.stringify(result))
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
    //     vm.list = resp.data.data1
    //     vm.initChart(vm.list)
    //   })
    // }
  }
}
</script>
