<template>
    <ExpandFold @callback="expandFn"></ExpandFold>
    {{ item.name }}
    <FileTree :item="item" :directoryFile="directoryFile"></FileTree>
</template>
  
<script setup lang="ts">
import { reactive } from 'vue';
import ExpandFold from './ExpandFold.vue';
import FileTree from './FileTree.vue';

let props = defineProps(['item'])

let directoryFile = reactive<directoryFile>({
    file: [],
    directory: []
});

let expandFn = async function (expand: boolean) {
    if (!expand) {
        directoryFile.directory = [];
        directoryFile.file = [];
        return;
    }
    let { directory, file } = await getDirectorFile(props.item);
    directoryFile.directory = directory;
    directoryFile.file = file;
    return;
}
async function getDirectorFile(directory: FileSystemDirectoryHandle): Promise<directoryFile> {
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
.h2 {
    margin-left: 15px;
}
</style>
  