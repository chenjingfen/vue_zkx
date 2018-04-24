<template>
<div :id="id"></div>
</template>

<script>
export default {
  props: ['id', 'data', 'title', 'color'],
  watch: {
    data: function () {
      this.$echarts.dispose(document.getElementById(this.id))
      let myChart = this.$echarts.init(document.getElementById(this.id))
      let data = this.data
      console.log('---------console.log(data)--------------------')
      console.log(data)
      myChart.setOption({
        title: {
          text: this.title,
          x: 'center',
          textStyle: {
            color: ['#fff']
          }
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'shadow'
          }
        },
        grid: {
          bottom: '3%',
          x: 80
        },
        toolbox: {
          feature: {
            saveAsImage: {backgroundColor: ['#032549']}
          }
        },
        xAxis: {
          show: false,
          type: 'value',
          boundaryGap: [0, 0.01]
        },
        color: [this.color],
        yAxis: {
          type: 'category',
          data: data.yData,
          nameTextStyle: {
            color: ['#fff']
          },
          axisLabel: {
            textStyle: {
              color: '#fff'
            }
          }
        },
        series: [
          {
            name: '访问来源',
            type: 'bar',
            data: data.data,
            label: {
              normal: {
                show: true,
                position: 'right'
              }
            },
            animationDelay: function (idx) {
              // 延迟动画效果
              return idx * 20
            },
            barMinHeight: 10
          }
        ],
        animationEasing: 'elasticOut',
        animationDelayUpdate: function (idx) {
          return idx * 5
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
