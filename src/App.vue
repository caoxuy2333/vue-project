<template>
  <div class="view">
    <div class="left">
      <button class="btn" @click="openFilePicker">选择文件夹</button>
      <div class="dir">
        <FileTree :directoryFile="directoryFile"></FileTree>
      </div>
    </div>
    <div class="right">
              
      <div class="tabs-list">
        <div class="tab-item" :class="item == tabs.current? 'active': ''" v-for="(item) in Object.keys(tabs.list)" :key="item">
          <label :for="item"> {{ item }}</label>
          <span @click="closeTab(item)" class="iconfont icon-RectangleCopy"></span>
        </div>
      </div>
      <br />
      <div v-for="(item) in Object.keys(tabs.list)" :key="item">
        <EditView :itemId="item" :item="tabs.list[item]" :current="tabs.current"></EditView>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive,  provide } from 'vue';
import FileTree from './components/FileTree.vue';
import EditView from './components/EditView.vue';

declare global {
  interface Window {
    showOpenFilePicker: Function,
    showDirectoryPicker: Function,
  }

  interface FileSystemFileHandle {
    entries: Function
  }
  interface FileSystemDirectoryHandle {
    entries: Function
  }

  interface directoryFile {
    file: Array<FileSystemFileHandle>
    directory: Array<FileSystemDirectoryHandle>
  }
}

interface tabItemObject{
  [val:string]: string 
}

interface tabItemProps {
  current: string
  list: tabItemObject
}

let directoryFile = reactive<directoryFile>({
  file: [],
  directory: []
}); 
let picker: FileSystemDirectoryHandle;
let tabs = reactive<tabItemProps>({
  current: '1',
  list: {}
})
 

// 选择文件
provide('updateFileContent', async function (content: string, file: FileSystemFileHandle) {
  let filePath = await picker.resolve(file);
  let path:string = filePath?.join('/') || '';
  if (tabs.list[path]) {
    tabs.current = path;
  } else { 
    tabs.current = path;
    tabs.list[path] = content
  } 
  console.log(tabs.list);
})

// 关闭单个标签页
function closeTab(item:any){
  delete tabs.list[item];
}

// 打开文件
async function openFilePicker() {
  let pickerFlod = await window.showDirectoryPicker()
  let { directory, file } = await getDirectorFile(pickerFlod)
  directoryFile.directory = directory;
  directoryFile.file = file;
  picker = pickerFlod;
}

// 打开文件夹
async function getDirectorFile(directory: FileSystemDirectoryHandle) {
  let directoryFile: directoryFile = {
    file: [],
    directory: []
  };
  for await (const [name, handle] of directory.entries()) {
    if (handle.kind === 'file') {
      const fileHandle = await directory.getFileHandle(name);
      directoryFile.file.push(fileHandle)
    } else {
      const directoryHandle = await directory.getDirectoryHandle(name);
      directoryFile.directory.push(directoryHandle)
    }
  }
  return directoryFile;
}
</script>

<style scoped>
.view {
  display: flex;
  height: 100vh;
  width: 100vw;

  .left {
    width: 250px;
    border: 1px solid #ccc;
    padding: 10px;
    margin: 10px;
    padding-left: 0;
    display: flex;
    flex-direction: column;

    .dir {
      overflow: auto;
    }
  }

  .right {
    flex: 1;
    margin: 10px;
    overflow: hidden;

    .tabs-list {
      display: flex;

      .tab-item {
        border: 1px solid #ccc;
        margin: 0 5px;

        :hover {
          background-color: #ccc;
        }
      }
    }
  }
}

.btn {
  margin-left: 10px;
}
</style>
