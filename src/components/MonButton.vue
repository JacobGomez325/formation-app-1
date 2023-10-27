<template>
  <button  :type="props.types" :class="`${props.styles} `">
    <slot></slot>
  </button>
</template>

<script setup>
const props = defineProps(['styles','types'])

</script>

<style lang="scss" scoped>

</style>