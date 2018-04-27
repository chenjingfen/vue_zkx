<template>
  <div class="pagination">
    <span class="prev">上一页</span>
    <span >{{page}}</span>
    <span class="next">下一页</span>
  </div>
</template>

<script>
export default {
  props: ['data'],
  data () {
    return {
    }
  },
  computed: {
    page: function () {
      console.log(this.data)
      return this.data
    }
  }
}
</script>

<style scoped>

</style>
