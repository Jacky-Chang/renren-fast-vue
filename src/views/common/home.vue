<template>
  <div class="mod-demo-echarts">
    <el-row :gutter="20">
      <el-col :span="24" v-if="false">
        <el-card>
          <div id="J_chartLineBox" class="chart-box"></div>
        </el-card>
      </el-col>
      <el-col :span="24">
        <el-card>
          <div id="ChartGraphicDailyVisits" class="chart-box"></div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import echarts from 'echarts'
  export default {
    data () {
      return {
        chartLine: null,
        chartGraphic: null
      }
    },
    mounted () {
      // this.initChartLine()
      this.initChartGraphic()
    },
    activated () {
      // 由于给echart添加了resize事件, 在组件激活时需要重新resize绘画一次, 否则出现空白bug
      if (this.chartLine) {
        this.chartLine.resize()
      }
    },
    methods: {
      // 折线图
      initChartLine () {
        var option = {
          'title': {
            'text': '折线图堆叠'
          },
          'tooltip': {
            'trigger': 'axis'
          },
          'legend': {
            'data': [ '邮件营销', '联盟广告', '视频广告', '直接访问', '搜索引擎' ]
          },
          'grid': {
            'left': '3%',
            'right': '4%',
            'bottom': '3%',
            'containLabel': true
          },
          'toolbox': {
            'feature': {
              'saveAsImage': { }
            }
          },
          'xAxis': {
            'type': 'category',
            'boundaryGap': false,
            'data': [ '周一', '周二', '周三', '周四', '周五', '周六', '周日' ]
          },
          'yAxis': {
            'type': 'value'
          },
          'series': [
            {
              'name': '邮件营销',
              'type': 'line',
              'stack': '总量',
              'data': [ 120, 132, 101, 134, 90, 230, 210 ]
            },
            {
              'name': '联盟广告',
              'type': 'line',
              'stack': '总量',
              'data': [ 220, 182, 191, 234, 290, 330, 310 ]
            },
            {
              'name': '视频广告',
              'type': 'line',
              'stack': '总量',
              'data': [ 150, 232, 201, 154, 190, 330, 410 ]
            },
            {
              'name': '直接访问',
              'type': 'line',
              'stack': '总量',
              'data': [ 320, 332, 301, 334, 390, 330, 320 ]
            },
            {
              'name': '搜索引擎',
              'type': 'line',
              'stack': '总量',
              'data': [ 820, 932, 901, 934, 1290, 1330, 1320 ]
            }
          ]
        }
        this.chartLine = echarts.init(document.getElementById('J_chartLineBox'))
        this.chartLine.setOption(option)
        window.addEventListener('resize', () => {
          this.chartLine.resize()
        })
      },
      initChartGraphic () {
        var dom = document.getElementById('ChartGraphicDailyVisits')
        var visitChart = echarts.init(dom)
        var dataAxis = ['点', '击', '柱', '子', '或', '者', '两', '指', '在', '触', '屏', '上', '滑', '动', '能', '够', '自', '动', '缩', '放']
        dataAxis = ['2017-08-09', '2017-08-10', '2017-08-11', '2017-08-12', '2017-08-13']
        var data = [220, 182, 191, 234, 290, 330, 310, 123, 442, 321, 90, 149, 210, 122, 133, 334, 198, 123, 125, 220]
        data = [['2017-08-09', 220], ['2017-08-10', 182], ['2017-08-11', 191], ['2017-08-12', 234], ['2017-08-13', 290]]

        var option = {
          title: {
            text: '官网日访问量',
            subtext: 'Daily visits to official website',
            x: 'center',
            y: 'top',
            textAlign: 'center',
            textStyle: {
              color: '#997B71'
            }
          },
          xAxis: {
            data: dataAxis,
            // type: 'time',
            axisLabel: {
              inside: false,
              // rotate: 15,
              textStyle: {
                color: '#999'
              }
            },
            axisTick: {
              show: false
            },
            axisLine: {
              show: false
            }
          },
          yAxis: {
            axisLine: {
              show: false
            },
            axisTick: {
              show: false
            },
            axisLabel: {
              textStyle: {
                color: '#999'
              }
            }
          },
          dataZoom: [
            {
              type: 'inside'
            }
          ],
          series: [
            {
              type: 'bar',
              itemStyle: {
                normal: {
                  // color: new echarts.graphic.LinearGradient(
                  //   0, 0, 0, 1,
                  //   [
                  //     {offset: 0, color: '#997B71'},
                  //     {offset: 0.5, color: '#925e4d'},
                  //     {offset: 1, color: '#903e23'}
                  //   ]
                  // )
                  color: '#997B71'
                },
                emphasis: {
                  // color: new echarts.graphic.LinearGradient(
                  //   0, 0, 0, 1,
                  //   [
                  //     {offset: 0, color: '#ac641e'},
                  //     {offset: 0.7, color: '#ac771e'},
                  //     {offset: 1, color: '#997B71'}
                  //   ]
                  // )
                  color: '#ac641e'
                }
              },
              data: data
            }
          ]
        }

        // Enable data zoom when user click bar.
        var zoomSize = 6
        visitChart.on('click', function (params) {
          console.log(dataAxis[Math.max(params.dataIndex - zoomSize / 2, 0)])
          visitChart.dispatchAction({
            type: 'dataZoom',
            startValue: dataAxis[Math.max(params.dataIndex - zoomSize / 2, 0)],
            endValue: dataAxis[Math.min(params.dataIndex + zoomSize / 2, data.length - 1)]
          })
        })
        if (option && typeof option === 'object') {
          visitChart.setOption(option, true)
        }
      }
    }
  }
</script>

<style lang="scss">
  .mod-demo-echarts {
    > .el-alert {
      margin-bottom: 10px;
    }
    > .el-row {
      margin-top: -10px;
      margin-bottom: -10px;
      .el-col {
        padding-top: 10px;
        padding-bottom: 10px;
      }
    }
    .chart-box {
      min-height: 400px;
    }
  }
</style>
