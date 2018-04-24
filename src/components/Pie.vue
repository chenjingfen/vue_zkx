<template>
<div :id="id"
></div>
</template>

<script>
export default {
  props: ['id', 'data', 'title'],
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
          trigger: 'item',
          formatter: '{a} <br/>{b} : {c} ({d}%)'
        },
        toolbox: {
          feature: {
            saveAsImage: {backgroundColor: ['#032549']}
          }
        },
        legend: {
          orient: 'vertical',
          left: 'left',
          data: data.legend,
          show: false
        },
        series: [
          {
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            center: ['50%', '60%'],
            data: data.data,
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(5, 0, 0, 0.5)'
              },
              normal: {
                color: function (params) {
                  var colors = [
                    '#4EC1FF', '#24B9A6',
                    '#6FC272', '#AECB4B',
                    '#EBAB30', '#E65F3F',
                    '#FFE987', '#CCD7B5',
                    '#EFD2C0', '#F5BDB0'
                  ]
                  return colors[params.dataIndex]
                }
              }
            }
          }
        ]
      })
    }
  }
}
</script>

<style scoped>

</style>
