<template>
  <div id="app" class="bg-dark text-light min-vh-100">
    <h1>Dog Ceo</h1>
    <router-view />
  </div>
</template>

<script>
import router from './router';

export default {
  name: 'App',
  router,
};
</script>

<style>
/* Fondo oscuro y estilo global */
body {
  font-family: Arial, sans-serif;
  background-color: #121212;
  color: #f5f5f5; /* Texto blanco */
  
}
h1, h2 {
  color: #ffffff; 
}

</style>
