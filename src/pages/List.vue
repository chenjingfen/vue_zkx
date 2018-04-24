<template>
    <main-layout>
        <tab
        :items="items"
        :activeItem="activeItem"
        :activeKey="activeKey"
        @activeClick="activeClick"
        @searchItem="searchItem"
        ></tab>
    </main-layout>
</template>

<script>
import MainLayout from '../components/Layout.vue'
import Tab from '../components/Tab.vue'

export default {
  components: {
    MainLayout,
    Tab
  },
  beforeCreate () {
    this.$http.get('/static/data/brand.json').then((response) => {
      this.items['deviceBrand'] = response.data
    })

    this.$http.get('/static/data/model.json').then((response) => {
      this.items['deviceModel'] = response.data
    })

    this.$http.get('/static/data/protocol.json').then((response) => {
      this.items['protocol'] = response.data
      this.activeItem = response.data
    })
  },
  data () {
    return {
      activeItem: [],
      items: {},
      activeKey: 'protocol'
    }
  },
  methods: {
    activeClick: function (key) {
      var items = this.items
      this.activeItem = items[key]
      this.activeKey = key
    },
    searchItem: function (item, key) {
      console.log(item, key)
      location.href = 'detailed?type=' + key + '&value=' + item
    }
  }
}
</script>
