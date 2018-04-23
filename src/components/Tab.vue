<template>
<div>
    <ul class="x-title" v-for="(item, index) in items" @click="tab(index)">
        <li :class="{active: activeIndex === index}">{{item.title}}</li>
    </ul>
    <div class="x-content">
        {{items[activeIndex].content}}
    </div>
</div>
</template>

<script>

</script>


<style scoped>

</style>