<template>
  <div>
    <div v-for="(item, index) in data"
         :key="index"
         class="index%2===0?'even':'odd'"
    >
      <div class="item-left">
        <a>{{item['ipInfo']['ip']}}</a>
        <span class="span-tag">{{item['portInfo']['deviceInfo']['deviceCategory']||'大类未知'}}</span>
        <span class="span-tag">{{item['portInfo']['deviceInfo']['deviceType']||'类型未知'}}</span>
        <span class="span-tag">{{item['portInfo']['deviceInfo']['deviceBrand']||'品牌未知'}}</span>
        <span class="span-tag">{{item['portInfo']['deviceInfo']['deviceModel']||'型号未知'}}</span>
        <label class="span-country">{{item['portInfo']['deviceInfo']['deviceLocation']['zhCountry']||'国家未知'}}</label>
        <label class="span-time">{{formatDate(item['lastModified'])}}</label>
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
