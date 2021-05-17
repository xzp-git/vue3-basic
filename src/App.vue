<template>
  <Modal :isOpen="flag" @close-modal="close">My Modal!!!!!!!</Modal>
  <button @click="open">打开</button>
  
  <div id="#app">
    <h2>45555555555</h2>
  </div>
  <!-- <h1>{{aaa}}</h1> -->
  <h1>{{b}}</h1>
  <h1>b.value.a:{{b.a}}</h1>
  <button type="button" @click="fn">++</button>
  <h1>{{fooRef}}</h1>

  <ul>
    <li v-for="item in arr" :key="item.age">item.name:{{item.name}},item.age:{{item.age}}</li>
  </ul>

  <button type="button"></button>
</template>

<script>
import Modal from "./components/Modal.vue";
import axios from "axios";
import {  customRef, reactive, toRef,  ref, watch } from 'vue'

function customRefFn (url, initVal) { 
  let data = initVal
  return customRef((track, trigger) => {
    setTimeout( function() {
      axios(url).then(res => {
        data = res.data
        trigger()
      })
    }, 2000) 
    return{
      get () {
        track()
        return data
      },
      set (newValue) {

        //通知Vue重新渲染
        trigger()
      }
    }
  })
}
export default {
  name: 'App',
  components: {
    Modal
  },
  setup () {
    let arr = customRefFn("/data.json", [])
    let flag = ref(false)
    const b = ref({a:5, k:10, o:4 })
    
    const fn = () => {
      b.value.a++
       console.log(b.value.a);
       
    }
    const open = () => {
      flag.value = true
    }

     const close = () => {
       flag.value = false
    }

    const state = reactive({
      foo: 1,
      bar: 2
    })

    const fooRef = toRef(state, 'foo')
    watch(() => {
      return b.value.a
    }, () => {
      console.log(b.value.a);
    })
    setTimeout(() => {
        fooRef.value++
        console.log(state.foo) // 2

        // state.foo++
        // console.log(fooRef.value) // 3
    }, 2000)
    return {
      fn,
      b,
      open,
      flag,
      close,
      state,
      fooRef,
      arr
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
