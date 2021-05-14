<template>
  <div id="#app">
    <h2>45555555555</h2>
  </div>
  <!-- <h1>{{aaa}}</h1> -->
  <h1>{{b}}</h1>
  <h1>b.value.a:{{b.a}}</h1>
  <button type="button" @click="fn">++</button>
</template>

<script lang="ts">

import {  computed, reactive, toRefs, onMounted,onUnmounted, onUpdated, onRenderTriggered, watch, ref, onErrorCaptured } from 'vue'

export default {
  name: 'App',
  setup () {
    const b = ref({a:5, k:10, o:4 })
    
    const fn = () => {
      b.value.a++
       console.log(b.value.a);
       
    }
    return {
      fn,
      b
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
