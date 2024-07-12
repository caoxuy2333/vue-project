<template>
  <section class="home" ref="sectionRef" v-show="itemId === current">
    <codemirror
      v-model="code"
      placeholder="Code goes here..."
      :style="codeHeight"
      :autofocus="true"
      :indent-with-tab="true"
      :tab-size="2"
      :extensions="extensions"
      @change="console.log('change', $event)"
    />
  </section>
</template>

<script setup lang="ts">
import { onMounted, reactive, ref } from 'vue'
import { Codemirror } from 'vue-codemirror'
import { javascript } from '@codemirror/lang-javascript'
const extensions = ref([javascript()])

let props = defineProps(['item', 'current', 'itemId'])
const code = ref(props.item)
const sectionRef = ref()
const codeHeight = reactive({
  height: ''
})

onMounted(function () {
  codeHeight.height = sectionRef.value.parentNode.parentNode.clientHeight - 50 + 'px'
})
</script>

<style scoped>
</style>
