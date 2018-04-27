<template>
  <div class="devices">
    <div v-for="(item, index) in data"
         :key="index"
         :class="index%2===0?'even':'odd'"
         class="item"
    >
      <div class="item-left">
        <div class="item-title"><a>{{item['ipInfo']['ip']}}</a></div>
        <span class="span-tag" :title="item['portInfo']['deviceInfo']['deviceCategory']||'大类未知'">{{item['portInfo']['deviceInfo']['deviceCategory']||'大类未知'}}</span>
        <span class="span-tag" :title="item['portInfo']['deviceInfo']['deviceType']||'类型未知'">{{item['portInfo']['deviceInfo']['deviceType']||'类型未知'}}</span>
        <span class="span-tag" :title="item['portInfo']['deviceInfo']['deviceBrand']||'品牌未知'">{{item['portInfo']['deviceInfo']['deviceBrand']||'品牌未知'}}</span>
        <span class="span-tag" :title="item['portInfo']['deviceInfo']['deviceModel']||'型号未知'">{{item['portInfo']['deviceInfo']['deviceModel']||'型号未知'}}</span>
        <div class="location"><label class="span-country" title="地理位置"><img src="../images/location.png"/>{{item['portInfo']['deviceInfo']['deviceLocation']['zhCountry']||'国家未知'}},{{item['portInfo']['deviceInfo']['deviceLocation']['zhProvince']||'省份未知'}},{{item['portInfo']['deviceInfo']['deviceLocation']['zhCity']||'城市未知'}}</label></div>
        <div><label class="span-time"><img src="../images/time.png"/>{{formatDate(item['lastModified'])}}</label></div>
      </div>
      <div class="item-right">
        <label class="protocol-port">{{item['portInfo']['protocol']||'协议未知'}}:{{item['portInfo']['port']||'端口未知'}}</label>
        <pre>{{item['portInfo']['banner']}}</pre>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  props: ['data'],
  methods: {
    formatDate: function (obj) {
      var date = new Date(obj)
      var y = 1900 + date.getYear()
      var m = '0' + (date.getMonth() + 1)
      var d = '0' + date.getDate()
      return y + '/' + m.substring(m.length - 2, m.length) + '/' + d.substring(d.length - 2, d.length)
    }
  }
}
</script>

<style scoped>
  pre{
    white-space: pre-line;
    max-height: 200px;
    width: 100%;
    overflow: auto;
    word-break: break-all;
  }
  .item-left{
    width: 25%;
    display: inline-block;
    vertical-align: top;
  }
  .item-right{
    width: 74%;
    display: inline-block;
  }
  .even{
    background: rgba(255, 255, 255, 0.2);
  }
  .odd{
    background: rgba(255, 255, 255, 0.1)
  }
  .item{
    padding: 10px;
    margin-bottom: 10px;
  }
  .item-title{
    color: #f27f02;
    padding-bottom: 10px;
  }
  .span-tag{
    width: 49%;
    display: inline-block;
    text-align: center;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .even .span-tag{
    background: rgba(158, 158, 158, 0.2);
  }
  .odd .span-tag{
    background: rgba(158, 158, 158, 0.5);
  }
  .location{
    margin-bottom: 5px;
    padding-bottom: 5px;
  }
  .span-country{
    background:#d9534f;
    padding: 0 5px 5px 5px;
  }
  .span-country img,.span-time img{
    vertical-align: bottom;
    margin-right: 5px;
  }
  .span-time{
    background:#337ab7;
    padding: 0 5px 5px 5px;
  }

</style>

<style>
  body::-webkit-scrollbar{
    display: none;
  }
  ::-webkit-scrollbar{
    width: 5px;
    height: 10px;
    border-radius: 10px;
    background-color: rgba(0,0,0,0);
  }
  ::-webkit-scrollbar-track{
    box-shadow: inset 0 0 5px rgba(0,0,0,0);
    background-color: rgba(0,0,0,0);
  }
  ::-webkit-scrollbar-button{
    height: 10px;
    background-color: #fff;
  }
  ::-webkit-scrollbar-thumb{
    border-radius: 20px;
    background-color: rgba(0,0,0,0);
    cursor: pointer;
    background-color: #048AE9;
  }
</style>
