<script setup>
import { ref, reactive, computed,watch, onMounted,watchEffect,watchPostEffect,onUpdated, onBeforeUpdate,onBeforeMount } from 'vue'
import HelloWorld from './components/HelloWorld.vue'
import Count from './components/Count.vue'
import DefineModel from './components/DefineModel.vue'

const state = ref({
  count: 0
})
const text = ref('测试defineModel')

watchPostEffect(() => {
  /* 在 Vue 更新后执行 */
  console.error('watchPostEffect',state.value.count)
})
watch([()=> state.value.count], (count)=>{
  console.error('watch immediate: true')
},{immediate: true})

watchEffect(() => {
  console.error('watchEffect',state.value.count)
})

watch([()=> state.value.count], (count)=>{
  console.error('watch immediate: true')
},{immediate: true})

onBeforeMount(() => {
  console.error('beforeMount')
})

onMounted(() => {
  console.error('mounted')
})

onBeforeUpdate(() => {
  console.error('beforeUpdate')
})

onUpdated(() => {
  console.error('updated')
})

const incrementCb = (count,a,b) => {
  console.error('incrementCb',count)
  console.error('incrementCb',a)
  console.error('incrementCb',b)
}


</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg= "You did it!"/>
    </div>

    <Count :initialCount="999" @incrementCb="incrementCb"/>
    <h1> {{text}} </h1>
    <DefineModel v-model:title.trimAll="text"></DefineModel>
  </header>

</template>

<style scoped>
header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
