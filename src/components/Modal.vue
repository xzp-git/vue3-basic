<template>
  <teleport to= "#modal">
     <div id="center" v-if="isOpen">
       <h2><slot>this is a modal</slot></h2>
       <button @click="closeModal"><h1>关闭</h1></button>
     </div>
  </teleport>
</template>

<script lang = "ts" >
import { defineComponent  } from "vue";
export default defineComponent({
  props: {
      isOpen: Boolean,
  },
  emits:{
   "close-modal":null
  },
  setup(props,context){
      
      const closeModal =  () => {
          context.emit("close-modal")
      }

      return{
          closeModal
      }
  }
})
</script>

<style>
#center{
    width: 200px;
    height:200px;
    border: 1px solid #000;
}
</style>