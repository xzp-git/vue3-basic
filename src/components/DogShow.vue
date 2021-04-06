<template>
  <img :src="result && result.message">
</template>

<script lang="ts">
import axios from "axios";
import {  defineComponent  } from "vue";
export default defineComponent({
 async setup(){
   const res = await axios.get("https://dog.ceo/api/breeds/image/random")
   return {
       result: res.data
   }
 }
})
</script>

<style>

</style>