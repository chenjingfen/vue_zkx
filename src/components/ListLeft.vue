<template>
  <div>
    <div v-if="data && Object.keys(data).length > 0" class="left-dotted">{{title}}(TOP5)</div>
    <div v-for="(value, key, index) in data"
           :key="index"
           class="left-item"
    >
      {{key}}
      <span class="pull-right">{{value}}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ['title', 'data']
}
</script>

<style scoped>
  .left-item{
    width: 100%;
  }
  .pull-right{
    float: right;
  }
</style>
