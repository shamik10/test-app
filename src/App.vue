<template>
  <div class="app">
    <HelloWorld />
  </div>
</template>


<script setup>
  import HelloWorld from './components/HelloWorld.vue';
  import { ref } from 'vue';
  
</script>


<style scoped>
  .app {
    width: 80%;
    display: flex;
    flex-direction: column;
    justify-self: center;
    margin-top: 40px;

  }
</style>
