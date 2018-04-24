<template>
    <main-layout>
        <div class="report-container">
          <div class="left">
            <pie id="left-top"
                 :data="deviceService"
                 title="服务"
            ></pie>
            <pie id="left-center"
                 :data="port"
                 title="端口"
            ></pie>
            <bar id="left-bottom"
                 :data="protocol"
                 title="协议"
                 color="#FF9849"
            ></bar>
          </div>
          <div class="main">
            <div class="main-center">
              <world-map id="main-map"
                         :data="country"
              ></world-map>
              <ol class="top10">
                <li v-for="(item,index) in top10"
                     :key="index"
                >{{item.name}}<span class="pull-right">{{item.value}}</span></li>
              </ol>
            </div>
            <div class="main-bottom">
              <bar id="main-left"
                   :data="deviceType"
                   title="类型"
                   color="#FFF950"
              ></bar>
              <bar id="main-center"
                   :data="deviceBrand"
                   title="品牌"
                   color="#B1FF4F"
              ></bar>
              <bar id="main-right"
                   :data="deviceModel"
                   title="型号"
                   color="#4EC1FF"
              ></bar>
            </div>
          </div>
        </div>
    </main-layout>
</template>

<script>
import MainLayout from '../components/Layout.vue'
import Pie from '../components/Pie.vue'
import Bar from '../components/Bar.vue'
import WorldMap from '../components/Map.vue'

export default {
  components: {
    MainLayout,
    Pie,
    Bar,
    WorldMap
  },
  beforeMount () {
    console.log('-----------beforeCreate-----------------')
    var json = {
      _from: 0,
      _size: 0,
      _minimumShould: 1,
      _accuracy: {
        _must: [
          {'portInfo.deviceInfo.deviceCategory': 'ICS'},
          {lastModified: '1520784000000~' + new Date().getTime()}
        ]
      },
      _aggBy: {
        'portInfo.deviceInfo.deviceModel': 100,
        'portInfo.deviceInfo.deviceLocation.country': 1000,
        'portInfo.deviceInfo.deviceCategory': 100,
        'portInfo.protocol': 100,
        'portInfo.deviceInfo.deviceType': 100,
        'portInfo.deviceInfo.deviceBrand': 100,
        'portInfo.port': 100,
        'portInfo.deviceInfo.deviceService': 100
      }
    }
    this.$http.post('/api/port_info', json).then((res) => {
      let data = res.data
      this.parseData(data)
    }).catch((err) => {
      console.log(err)
    })
  },
  data () {
    return {
      deviceService: {},
      port: {},
      protocol: {},
      deviceType: {},
      deviceBrand: {},
      deviceModel: {},
      country: {},
      top10: [],
      count: {
        brandCount: 44,
        modelCount: 3991,
        typeCount: 15
      }
    }
  },
  computed: {
  },
  methods: {
    parseData: function (allData) {
      let agg = allData['_aggregation']

      this.count['allCount'] = allData['_total']

      let portData = this.parsePie(agg['portInfo.port'])
      this.count['portCount'] = portData.count
      this.port = portData.data

      let serviceData = this.parsePie(agg['portInfo.deviceInfo.deviceService'])
      this.count['serviceCount'] = serviceData.count
      this.deviceService = serviceData.data
      this.deviceType = this.parseBar(agg['portInfo.deviceInfo.deviceType'])
      this.deviceBrand = this.parseBar(agg['portInfo.deviceInfo.deviceBrand'])
      this.deviceModel = this.parseBar(agg['portInfo.deviceInfo.deviceModel'])
      this.protocol = this.parseBar(agg['portInfo.protocol'])
      this.country = this.parseMap(agg['portInfo.deviceInfo.deviceLocation.country'])
    },
    parsePie: function (data) {
      let cou = 0
      let others = 0
      let pieData = {data: [], legend: []}
      for (let key in data) {
        cou++
        if (pieData['data'].length < 9) {
          pieData['data'].push({name: key, value: data[key]})
          pieData['legend'].push(key)
        } else {
          others = others + ((Number(data[key])) ? Number(data[key]) : 0)
        }
      }
      if (others > 0) {
        pieData['data'].push({name: '其它', value: others})
        pieData['legend'].push('其它')
      }
      return {
        count: cou,
        data: pieData
      }
    },
    parseBar: function (data) {
      let others = 0
      let barData = {yData: [], data: []}
      for (let key in data) {
        if (barData['data'].length < 9) {
          barData['data'].push(data[key])
          barData['yData'].push(key)
        } else {
          others = others + ((Number(data[key])) ? Number(data[key]) : 0)
        }
      }
      if (others > 0) {
        barData['data'].push(others)
        barData['yData'].push('其它')
      }
      return barData
    },
    parseMap: function (data) {
      let allData = []
      for (let key in data) {
        allData.push({name: key, value: data[key]})
        if (this.top10.length < 10) {
          this.top10.push({name: key, value: data[key]})
        }
      }
      return allData
    }
  }
}
</script>

<style scoped>
  .report-container{
    display: flex;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
  .left{
    width: 25%;
  }
  #left-center,#left-top{
    height: 33.333333%;
    width: 100%;
    border-bottom: 1px solid rgba(158, 158, 158, 0.72);
  }
  #left-bottom{
    height: 33.333333%;
    width: 100%;
    border-right: 1px solid rgba(158, 158, 158, 0.72);
  }

  .main{
    width: 75%;
    height: 100%;
  }
  .main-center{
    height: 66.666666%;
    width: 100%;
    border-bottom: 1px solid rgba(158, 158, 158, 0.72);
    border-left: 1px solid rgba(158, 158, 158, 0.72);
  }
  #main-map{
    height: 100%;
    width: 100%;
    display: inline-block;
  }
  .top10{
    position: absolute;
    width: 14%;
    height: 20%;
    bottom: 35%;
    border-top: 1px solid rgba(158, 158, 158, 0.72);
    border-right: 1px solid rgba(158, 158, 158, 0.72);
    border-top-right-radius: 10px;
    padding: 20px;
    margin: 0;
  }
  .top10 li{
    margin-left: 30px;
  }
  .top10 .pull-right{
    float: right;
  }
  .main-bottom{
    height: 33.333333%;
    width: 100%;
  }
  #main-left,#main-center{
    height: 100%;
    width: 33.333333%;
    border-right: 1px solid rgba(158, 158, 158, 0.72);
    display: inline-block;
  }
  #main-right{
    height: 100%;
    width: 32.5%;
    display: inline-block;
   }
</style>
