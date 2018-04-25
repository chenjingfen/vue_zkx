<template>
  <main-layout>
   <img src="../images/404.png" />
    <p class="p-title">您要找的页面不存在</p>
    <p>可能是因为您的链接地址有误或页面被删除。</p>
    <a href="/list">返回列表页面</a>
  </main-layout>
</template>

<script>
import MainLayout from '../components/Layout.vue'

console.log('404')
export default {
  components: {
    MainLayout
  }
}
</script>

<style scoped>
  img{
    margin-left: 37%;
    margin-top: 5%;
    width: 24%;
  }
  p{
    text-align: center;
  }
  .p-title{
    font-size: 20px;
  }
  a{
    margin-left: 47%;

  }
  a:link{
    color: aqua;
  }
  a:visited{
    color: aqua;
  }
  a:hover{
    color: orange;
  }
  a:active{
    color: orange;
  }
</style>
