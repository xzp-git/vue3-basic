<template>
  <div id="#app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1>{{ count }}</h1>
    <h1>{{ doubleCount }}</h1>
    <Modal :isOpen="flag" @close-modal="close">My Modal!!!!!!!</Modal>
    <button @click="open">打开</button>
    <ul>
      <li v-for="number in numbers" :key="number"> <h1>{{number}}</h1> </li>
    </ul>
    <h1>{{person.name}}</h1>
    <button @click="increase">👍+1</button>
    <h1 v-if="loading" >Loading...</h1>
    <img v-if="loaded" :src="result[0].url" >
    <br>
    <h1>{{titles}}</h1>
    <button @click="updateTitle" >更新title</button>
    <h1>X:{{x}},Y:{{y}}</h1>
  </div>
</template>

<script lang="ts">
import {  computed, reactive, toRefs, onMounted,onUnmounted, onUpdated, onRenderTriggered, watch, ref } from "vue"
import updateMousePosition from "./hooks/updateMousePosition";
import useUrlLoader from "./hooks/useUrlLoader";
import Modal from "./components/Modal.vue";
interface dataProps  {
  count:number,
  increase:() => void,
  doubleCount:number,
  numbers:number[],
  person:{name?:string},
}
interface DogResult {
  message: string;
  status:string; 
}

interface CatResult {
  id:string,
  url: string,
  width:number,
  height:number
}
export default {
  name: 'App',
  components: {
    Modal,
  },
  setup (  ) {
    // const count = ref(0)
    // const increase = () => {
    //   count.value++
    // }
    // const doubleCount = computed(()=> {
    //   return count.value * 2
    // })
    // return{
    //   count,
    //   increase,
    //   doubleCount
    // }
    
    
    
    onUpdated(() => {
      console.log("updated");
      
    })
    onRenderTriggered((event) => {
      console.log(event);
      
    })
    const data: dataProps = reactive({
      count:0,
      increase:() =>{ data.count++},
      doubleCount:computed(() => data.count * 2),
      numbers:[0,1,2],
      person:{}
    })
    data.numbers[0] = 5;
    data.person.name = "xzp"
    const refData = toRefs(data)
    const titles = ref("update")
    const updateTitle = ()=> {
          titles.value += "hello"
        
    }
    watch([titles, () =>data.count],(newval,oldval)=>{
      console.log("new",newval);
      console.log("old",oldval);
      document.title = titles.value + data.count 
    })

    const {x, y} = updateMousePosition()
    
    const { loading, loaded, result,error } = useUrlLoader<CatResult[]>("https://api.thecatapi.com/v1/images/search?limit=1")//https://dog.ceo/api/breeds/image/random
    watch(result,() => {
      if (result.value) {
        console.log("value", result.value[0].url);
      }
       
    }) 

    const flag = ref(false)
    const open = () => {
      flag.value = true
    }
    const close = () => {
      flag.value = false
    }
    return {
      ...refData,
      titles,
      updateTitle,
      x,
      y,
      loading,
      loaded,
      result,
      error,
      flag,
      open,
      close
    }
    
  } 
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
