<template>
  <span v-if="expand" @click="expandFn" class="iconfont icon-icon-test"></span>
  <span v-else @click="expandFn" class="iconfont icon-icon-test1"></span>
</template>

<script setup lang="ts">
import { ref } from 'vue'
let emit = defineEmits(['callback'])

let expand = ref<boolean>(false)
let expandFn = function (): undefined {
  expand.value = !expand.value
  emit('callback', expand.value)
}
</script>

<style scoped>
span {
  cursor: pointer;
}
</style>
