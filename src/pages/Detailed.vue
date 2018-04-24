<template>
  <main-layout>
    <p>detailed</p>
  </main-layout>
</template>

<script>
import MainLayout from '../components/Layout.vue'

export default {
  components: {
    MainLayout
  },
  beforeCreate () {
    let searches = location.search.replace('?', '').split('&')
    if (searches.length === 1) {
      location.href = 'list'
    } else {
      let type = searches[0].substr(searches[0].indexOf('=') + 1, searches[0].length)
      let value = searches[1].substr(searches[1].indexOf('=') + 1, searches[1].length)
      value = value.replace(/%20/g, ' ')
      console.log(type, value)
    }
  }
}
</script>
