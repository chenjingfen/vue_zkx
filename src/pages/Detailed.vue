<template>
  <main-layout>
   <div class="detailed-container">
     <div class="left">
       <div class="input-btn">
         <input v-model="query" @keyup.13="submitClick"/>
         <button class="submit-btn" @click="submitClick">搜索</button>
       </div>
       <div class="left-dotted">总数</div>
       <label class="count">{{data['count'].toLocaleString()}}</label>
       <list-left :data="data['agg']['portInfo.deviceInfo.deviceLocation.zhCountry']"
                  title="国家"
       ></list-left>
       <list-left :data="data['agg']['portInfo.protocol']"
                  title="服务"
       ></list-left>
       <list-left :data="data['agg']['portInfo.port']"
                  title="端口"
       ></list-left>
       <list-left :data="data['agg']['portInfo.deviceInfo.deviceType']"
                  title="类型"
       ></list-left>
       <list-left :data="data['agg']['portInfo.deviceInfo.deviceBrand']"
                  title="品牌"
       ></list-left>
       <list-left :data="data['agg']['portInfo.deviceInfo.deviceModel']"
                  title="型号"
       ></list-left>
     </div>
     <div class="right">
       <key-item :data="queryItem"
                 @handleDelete="handleDelete"
       ></key-item>
       <list-right :data="data['list']"></list-right>
       <pagination :data="page"
                   @handleSearch="handleSearch"
       ></pagination>
     </div>
   </div>
  </main-layout>
</template>

<script>
import MainLayout from '../components/Layout.vue'
import ListLeft from '../components/ListLeft.vue'
import ListRight from '../components/ListRight.vue'
import KeyItem from '../components/KeyItem.vue'
import Pagination from '../components/Pagination.vue'

export default {
  components: {
    MainLayout,
    ListLeft,
    ListRight,
    KeyItem,
    Pagination
  },
  beforeCreate () {
    let searches = location.search.replace('?', '').split('&')
    if (searches.length === 1) {
      location.href = 'list'
    }
  },
  created () {
    this.submitClick()
  },
  data () {
    return {
      query: '',
      queryItem: '',
      queryListArray: {
        protocol: [],
        port: [],
        deviceModel: [],
        deviceBrand: [],
        deviceCategory: [],
        deviceType: [],
        zhCountry: [],
        zhProvince: [],
        zhCity: [],
        keyWords: []
      },
      data: {
        count: 0,
        agg: [],
        list: []
      },
      page: {
        currentPage: 0,
        allPage: 0
      },
      from: 0
    }
  },
  computed: {
    type: function () {
      let searches = location.search.replace('?', '').split('&')
      let type = searches[0].substr(searches[0].indexOf('=') + 1, searches[0].length)
      return type
    },
    value: function () {
      let searches = location.search.replace('?', '').split('&')
      let value = searches[1].substr(searches[1].indexOf('=') + 1, searches[1].length).replace(/%20/g, ' ')
      return value
    }
  },
  methods: {
    submitClick: function () {
      this.searchListAgg()
      this.from = 0
    },
    searchListAgg: function () {
      let query = this.searchData()
      this.$http.post('/api/port_info', query).then((res) => {
        let data = res.data
        this.parseData(data)
        console.log(data)
      }).catch((err) => {
        console.log(err)
      })
      if (this.from === 0) {
        let aggQuery = this.searchAggData(query)
        this.$http.post('/api/port_info', aggQuery).then((res) => {
          let data = res.data
          this.data.agg = data['_aggregation']
          console.log(data)
        }).catch((err) => {
          console.log(err)
        })
      }
    },
    parseData: function (data) {
      let _query = JSON.parse(data['_query'])
      let _total = data['_total']
      let _size = data['_size']
      let _from = _query['_from']

      this.data.count = _total
      this.queryItem = this.query = this.parseQuery(_query)
      this.updateQueryArray(this.queryItem)
      this.data.list = data['_data']
      this.page.currentPage = parseInt(_from / _size) + 1
      this.page.allPage = parseInt(_total / _size) + 1
    },
    parseQuery: function (query) {
      let keys = query._keyword._must
      let items = query._accuracy._must
      let _query = ''
      if (keys.length > 0) {
        keys.forEach((key) => {
          _query = _query + key + ','
        })
        _query = _query.substr(0, _query.length - 1)
      }
      if (items.length > 0) {
        if (_query.length > 0) {
          _query = _query + ','
        }
        items.forEach((item) => {
          for (let key in item) {
            console.log(key)
            _query = _query + key.substr(key.lastIndexOf('.') + 1, key.length) + ':' + item[key] + ','
          }
        })
        _query = _query.substr(0, _query.length - 1)
      }
      console.log(_query)
      return _query
    },
    searchData: function () {
      console.log('--------this.query-----------')
      console.log(this.query)
      let _query = {
        _from: this.from,
        _size: 10,
        _minimumShould: 2,
        _keyword: {
          _must: [
          ]
        },
        _accuracy: {
          _must: [
          ],
          _should: [
            {lastModified: '1520784000000~' + new Date().getTime()}
          ]
        }
      }
      if (this.query.length === 0) {
        switch (this.type) {
          case 'protocol' :
          {
            _query._accuracy._must.push({'portInfo.protocol': this.value})
            break
          }
          case 'deviceModel' :
          {
            _query._accuracy._must.push({'portInfo.deviceInfo.deviceModel': this.value})
            break
          }
          case 'deviceBrand' :
          {
            _query._accuracy._must.push({'portInfo.deviceInfo.deviceBrand': this.value})
            break
          }
        }
        return _query
      } else {
        let items = this.query.split(',')
        items.forEach((item) => {
          if (item.indexOf(':') > -1) {
            let key = item.substr(0, item.indexOf(':'))
            let value = item.substr(item.indexOf(':') + 1, item.length)
            console.log(key, value)
            switch (key) {
              case 'protocol' :
              {
                _query._accuracy._must.push({'portInfo.protocol': value})
                break
              }
              case 'deviceModel' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceModel': value})
                break
              }
              case 'deviceBrand' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceBrand': value})
                break
              }
              case 'port' :
              {
                _query._accuracy._must.push({'portInfo.port': value})
                break
              }
              case 'deviceCategory' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceCategory': value})
                break
              }
              case 'deviceType' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceType': value})
                break
              }
              case 'zhCountry' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceLocation.zhCountry': value})
                break
              }
              case 'zhProvince' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceLocation.zhProvince': value})
                break
              }
              case 'zhCity' :
              {
                _query._accuracy._must.push({'portInfo.deviceInfo.deviceLocation.zhCity': value})
                break
              }
            }
          } else {
            _query._keyword._must.push(item)
          }
        })
        return _query
      }
    },
    searchAggData: function (query) {
      let aggBy = {
        'portInfo.port': 5,
        'portInfo.protocol': 5,
        'portInfo.deviceInfo.deviceType': 5,
        'portInfo.deviceInfo.deviceModel': 5,
        'portInfo.deviceInfo.deviceLocation.zhCountry': 5,
        'portInfo.deviceInfo.deviceBrand': 5
      }
      query['_aggBy'] = aggBy
      return query
    },
    updateQueryArray: function (queryItem) {
      for (let item in this.queryListArray) {
        this.queryListArray[item] = []
      }
      queryItem.split(',').forEach((item) => {
        let index = item.indexOf(':')
        if (index > -1) {
          let key = item.substr(0, index)
          let value = item.substr(index + 1, item.length)
          if (this.queryListArray[key] && this.queryListArray[key].indexOf(value) === -1) {
            this.queryListArray[key].push(value)
          }
        } else {
          if (this.queryListArray.keyWords.indexOf(item) === -1) {
            this.queryListArray.keyWords.push(item)
          }
        }
      })
    },
    handleDelete: function (item) {
      this.query = this.query.replace(item, '')
      this.query = this.query.split(',').filter((item) => item.length > 0).join(',')
      if (this.query.length === 0) {
        this.query = '*'
      }
      this.submitClick()
    },
    handleSearch: function (page) {
      this.from = (page - 1) * 10
      this.searchListAgg()
    }
  },
  watch: {
    query: function () {
      console.log('改变啦')
    }
  }
}
</script>

<style scoped>
  .detailed-container{
    width: 80%;
    padding-left: 10%;
    padding-right: 10%;
    padding-top: 20px;
  }
  .left{
    width: 20%;
    display: inline-block;
  }
  .input-btn{
    width: 100%;
    position: relative;
    display: table;
    border-collapse: separate;
  }
  .input-btn input{
    height: 30px;
    display: table-cell;
    position: relative;
    z-index: 2;
    float: left;
    width: calc(100% - 70px);
    margin-bottom: 0;
  }
  .input-btn button{
    top: 0!important;
    background: #337ab7;
    color: #fff;
    padding: 6px 12px;
    font-size: 14px;
    font-weight: 400;
    line-height: 1;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    width: 60px;
    height: 36px;
    border-left: none;
  }
  .count{
    font-size: 20px;
    font-weight: 500;
  }
  .right{
    width: calc(80% - 20px);
    display: inline-block;
    float: right;
    margin-left: 20px;
    margin-bottom: 100px;
  }
</style>

<style>
  .left-dotted{
    border-bottom: 1px dashed #999;
    width: 100%;
    line-height: 20px;
    font-size: 15px;
    color: #999;
    text-transform: uppercase;
    margin-bottom: 8px;
    margin-top: 15px;
  }
</style>
