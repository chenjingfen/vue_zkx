<template>
  <div>
    <label v-for="(item, index) in data.split(',')"
           :key="index"
    >{{item}}</label>
  </div>
</template>

<script>
export default {
  props: ['data']
}
</script>
