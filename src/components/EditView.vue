<template>
    <input type="radio" name="pure-nav" :id="itemId" :checked="itemId == current">
    <section class="home">
        <codemirror
            v-model="code"
            placeholder="Code goes here..."
            :style="{ height: '200px' }"
            :autofocus="true"
            :indent-with-tab="true"
            :tab-size="2"
            :extensions="extensions"
            @change="console.log('change', $event)"
        />
    </section>
</template>
  
<script setup lang="ts">
import { ref } from 'vue';
import { Codemirror } from 'vue-codemirror';
import { javascript } from '@codemirror/lang-javascript';
const extensions = ref([javascript()])

let props = defineProps(["item", "current", "itemId"]);
const code = ref(props.item)

</script>
  
<style scoped>
input[name="pure-nav"] {
     display: none;
 }

 input[name="pure-nav"]+section {
     display: none;
 }

 input[name="pure-nav"]:checked+section {
     display: block;
     -moz-transform: rotateX(0deg);
     -ms-transform: rotateX(0deg);
     -webkit-transform: rotateX(0deg);
     transform: rotateX(0deg);
     /* z-index: 6; */
     opacity: 1;
 }
</style>