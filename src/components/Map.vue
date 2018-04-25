<template>
<div :id="id"></div>
</template>

<script>

export default {
  props: ['id', 'data'],
  watch: {
    data: function () {
      this.$http.get('/static/data/world.json').then((response) => {
        this.$echarts.registerMap('world', response.data)
        this.$echarts.dispose(document.getElementById(this.id))
        let myChart = this.$echarts.init(document.getElementById(this.id))
        let data = this.data
        console.log('---------country--------------------')
        console.log(data)
        myChart.setOption({
          title: {
            text: '',
            left: 'center',
            top: 'top',
            textStyle: {
              color: ['#fff']
            }
          },
          tooltip: {
            trigger: 'item'
          },
          toolbox: {
            feature: {
              saveAsImage: {backgroundColor: ['#032549']}
            }
          },
          visualMap: {
            min: 0,
            max: data[0] ? data[0]['value'] : 30000,
            text: ['High', 'Low'],
            realtime: false,
            calculable: true,
            inRange: {
              color: ['lightskyblue', 'yellow', 'orangered']
            },
            show: false
          },
          series: [
            {
              name: '国家分布',
              type: 'map',
              mapType: 'world',
              roam: true,
              itemStyle: {
                emphasis: {
                  label: {show: true}
                }
              },
              data: data
            }
          ]
        })
      })
    }
  }
}
</script>

<style scoped>

</style>
