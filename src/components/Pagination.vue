<template>
  <div class="pagination">
    <span class="prev"
          :class="data.currentPage===1?'click-disabled':''"
          @click="clickPage(data.currentPage-1)"
    >上一页</span>
    <span v-for="(item,index) in page"
          :key="index"
          :class="item===data.currentPage?'currentPage':''"
          @click="clickPage(item)"
    >{{item}}</span>
    <span class="next"
          :class="data.currentPage===data.allPage?'click-disabled':''"
          @click="clickPage(data.currentPage+1)"
    >下一页</span>
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
      let pages = []
      let visiblePages = 7
      let currentPage = this.data.currentPage
      let totalPages = this.data.allPage

      if (visiblePages > totalPages) {
        visiblePages = totalPages
      }
      let half = Math.floor(visiblePages / 2)
      let start = currentPage - half + 1 - visiblePages % 2
      let end = currentPage + half
      if (start < 1) {
        start = 1
        end = visiblePages
      }
      if (end > totalPages) {
        end = totalPages
        start = 1 + totalPages - visiblePages
      }

      let itPage = start
      while (itPage <= end) {
        pages.push(itPage)
        itPage++
      }
      return pages
    }
  },
  methods: {
    clickPage: function (page) {
      if (page < 1 || page === this.data.currentPage || page > this.data.allPage) {
        return
      } else {
        this.$emit('handleSearch', page)
      }
      console.log(page)
    }
  }
}
</script>

<style scoped>
  .pagination{
    text-align: center;
  }
  .prev{
    margin-right: -5px;
    border-top-left-radius: 4px;
    border-bottom-left-radius: 4px;
  }
  .click-disabled{
    cursor: not-allowed;
  }
  .next{
    margin-left: -5px;
    border-right: 1px solid;
    border-top-right-radius: 4px;
    border-bottom-right-radius: 4px;
  }
  span{
    padding: 5px 10px;
    color: #777;
    cursor: pointer;
    background-color: #fff;
    border-color: #ddd;
    border-left: 1px solid;
  }
  .currentPage{
    background-color: #048AE9;
    color: #fff;
  }
</style>
