<script setup>
import { computed, ref } from 'vue';
import Home from './pages/Home.vue';
import LoginPage from './pages/LoginPage.vue';
import NotFound from './pages/NotFound.vue';

  const routes = {
    '/': Home,
    '/login': LoginPage
  };
  const currentPath = ref(window.location.hash);

  window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash;
  });

  const currentView = computed(() => {
    return routes[currentPath.value.slice(1) || '/login'] || NotFound;
  });
</script>

<template>
  <div class="app-container">
    <component :is="currentView"></component>
  </div>
</template>

<style scoped src="./assets/css/app.css">
</style>