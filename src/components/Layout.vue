<template>
<div>
  <header>
      <img src="../images/logo.png">
      <title>全球鹰关键基础设施主动识别子系统</title>
  </header>
  <div class="container">
    {{msg}}
    <slot></slot>
  </div>
<footer>底部</footer>
</div>
</template>

<script>
export default {
  name: 'MainLayout',
  data () {
    return {
      msg: 'helloWorld'
    }
  },
  methods: {}
}

</script>

<style scoped>
  html,body{
    height: 100%;
    width: 100%;
    margin: 0;
  }
  header{
  position: fixed;
  top:0;
  height: 50px;
  background: #000;
  width: 100%;
 }
  header img{
      height: 80%;
  }
  footer{
    position: fixed;
    bottom: 0;
    height: 30px;
  }
  .container{
    height: 100%;
    width: 100%;
    padding-top: 50px;
    padding-bottom: 50px;
  }
</style>
