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
    <div class="tab-content">
      <div class="dd-item dd"
           v-for="(item,index) in activeItem"
           :key="index"
      >
        <div class="dd dd-key"
             :data-type="activeKey"
             :title="item['key']"
             @click="searchItem(item['key'],activeKey)"
        >{{item['key']}}</div>
        <div class="dd dd-value">{{item['doc_count']}}</div>
      </div>
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
      console.log(key)
      switch (key) {
        case 'deviceBrand' :
          return '品牌'
        case 'protocol' :
          return '协议'
        case 'deviceModel':
          return '型号'
      }
    },
    searchItem: function (item, key) {
      this.$emit('searchItem', item, key)
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
    border-bottom: 1px solid transparent;
  }
  .tab-content{
    display: flex;
    flex-wrap: wrap;
  }
  .dd{
    display: inline-block;
    font-size: 12px;
    line-height: 24px;
    height: 24px;
    text-align: center;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    word-break: keep-all;
  }
  .dd-item{
    flex-basis: 16%;
    margin-bottom: 5px;
    transition: 1s;
  }
  .dd-item:hover{
    box-shadow: 6px 5px 20px #fff;
  }
  .dd-key{
    width: calc(66.66666667% - 20px);
    background: #3d3d3d;
    text-transform: uppercase;
    cursor: pointer;
    margin-left: 20px;
  }
  .dd-value{
    width: 33.33333333%;
    background: #3fa9f5;
  }
</style>
