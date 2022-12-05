<script setup lang="ts">
import {RouterView} from 'vue-router'
import Header from './components/HeaderComponent.vue'
</script>

<template>
  <div class="container my-12 mx-auto px-4 md:px-12">
    <Header />
    <RouterView/>
  </div>
</template>

<style scoped>

</style>
