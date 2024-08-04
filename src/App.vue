<!-- src/App.vue -->
<script setup>

import TodoList1 from './components/todoList1.vue';
import Shopcar from './components/shopcar.vue';


</script>

<template>
  <div id="app">
 
  
    <todoList1/>
    <shopcar/>

  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>