<template>
  <div class="keys-item">
    <label v-for="(item, index) in data.split(',')"
           :key="index"
    >{{item}}<img src="../images/delete.png"
                  @click="handleDelete(item)"
    /></label>
  </div>
</template>

<script>
export default {
  props: ['data'],
  methods: {
    handleDelete: function (item) {
      this.$emit('handleDelete', item)
    }
  }
}
</script>

<style scoped>
  .keys-item{
    padding: 10px 0;
  }
  .keys-item label{
    padding: 5px;
    background: #048ae9;
    margin-right: 5px;
  }
  .keys-item img{
    cursor: pointer;
    vertical-align: inherit;
    margin-left: 3px;
  }

</style>
