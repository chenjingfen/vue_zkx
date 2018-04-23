<template>
<div>
    <ul>
        <li
          v-for="(value,key,index) in items"
          :key="index"
          @click="handleClick(key)"
          :class="key===activeKey?'active':''"
        >{{getKeyName(key)}}</li>
    </ul>
    <div class="x-content">
    <span v-for="(item,index) in activeItem"
          :key="index"
    >{{item}}</span>
    </div>
</div>
</template>

<script>
export default {
  props: ['items', 'activeKey', 'activeItem'],
  computed: {
  },
  methods: {
    handleClick: function (key) {
      console.log(key)
      this.$emit('activeClick', key)
    },
    getKeyName: function (key) {
      switch (key) {
        case 'deviceBrand' :
          return '品牌'
        case 'deviceType' :
          return '类型'
        case 'deviceModel':
          return '型号'
      }
    }
  }
}
</script>

<style scoped>
  ul{
    height: 30px;
    line-height: 30px;
    list-style: none;
  }
li{
  width: fit-content;
  position: relative;
  cursor: pointer;
  display: inline-block;
  border: 1px solid #d7dde4;
  padding: 4px 6px;
  margin-right: 5px;
}
  li.active{
    color: #3399ff;
    border-top:1px solid #3399ff;
    border-bottom: 1px solid #fff;
  }
</style>
