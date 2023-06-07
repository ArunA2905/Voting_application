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
  <h1>Welcome To Voting App</h1>
  <div>
    <ul >
      <li><a href="#/">Home</a></li>
      <li><a href="#/login">LoginPage</a></li>
      <li><a href="#/not-found">PageNotFound</a></li>
    </ul>
  </div>

  <div>
    <component :is="currentView"></component>
  </div>
</template>