<template>
    <main-layout>
        <tab
        :items="items"
        :activeItem="activeItem"
        :activeKey="activeKey"
        @activeClick="activeClick"></tab>
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
  created () {
    this.$http.get('/static/data/data.json').then((response) => {
      console.log(response.data)
      console.log(JSON.stringify(response.data))
      this.items = response.data
      this.activeItem = response.data['deviceBrand']
    })
  },
  data () {
    return {
      activeItem: [],
      items: '',
      activeKey: 'deviceBrand'
    }
  },
  methods: {
    activeClick: function (key) {
      var items = this.items
      this.activeItem = items[key]
      this.activeKey = key
    }
  }
}
</script>
