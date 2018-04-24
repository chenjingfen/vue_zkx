<template>
<div class="main-container">
  <header>
      <img src="../images/logo.png">
      <span class="title">全球鹰关键基础设施主动识别子系统</span>
  </header>
  <div class="container">
    <slot></slot>
  </div>
<footer>©中国科学院信息工程研究所，2018年</footer>
</div>
</template>

<script>
export default {
  name: 'MainLayout'
}

</script>

<style scoped>
  header{
  position: fixed;
  top:0;
  height: 50px;
  background: #000;
  width: 100%;
 }
  header img{
      width: 40px;
      margin-top: 5px;
      margin-left: 10px;
      margin-right: 10px;
  }
  header .title{
      color: #128dc8!important;
      font-size: 24px;
      vertical-align: super;
  }
  footer{
    position: fixed;
    bottom: 0;
    height: 30px;
    color: #fff;
    background: #000;
    width: 100%;
    padding: .3rem 0 0 1rem;
    font-size: 14px;
  }
  .main-container{
    height: 100%;
  }
  .container{
    height: calc(100% - 100px);
    width: 100%;
    padding-top: 50px;
    padding-bottom: 50px;
  }
</style>

<style>
  html{
    background: url("../images/bg.png");
    color: #fff;
  }
</style>
