<template>
  <!-- 展示文件夹列表 -->
  <div class="h2" v-for="item in directoryFile.directory" :key="item">
    <FileTreeItem :item="item"></FileTreeItem>
  </div>
  <!-- 展示文件列表 -->
  <ul v-for="item in directoryFile.file" :key="item">
    <li @click="openFile(item)">
      {{ item.name }}
    </li>
  </ul>
</template>

<script setup lang="ts">
import { inject } from 'vue'
import FileTreeItem from './FileTreeItem.vue'
defineProps(['directoryFile', 'item'])

let updateFn: Function = inject('updateFileContent', () => 0)

let openFile = async function (item: any) {
  let file = await item.getFile()
  let reader = new FileReader()
  reader.readAsText(file, 'utf-8')
  reader.onload = function (readRes) {
    let res = readRes?.target?.result
    updateFn(res, item)
  }
}
</script>

<style lang="scss" scoped>
.h2 {
  margin-left: 13px;
  white-space: nowrap;
}

ul {
  padding-left: 17px;

  li {
    list-style: none;
  }
}
</style>
