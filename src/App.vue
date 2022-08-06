<template>
<div class="fondo">
  <Navbar />
  <Tarea />
</div>
</template>

<script>
import Navbar from './components/Navbar.vue';
import Tarea from './components/Tarea.vue';

export default {
  name: 'App',
  components: {
    Navbar,
    Tarea
  }
}
</script>

<style>
.fondo {
  background-image: url("./assets/fondo.jpg");
  height: 100vh;
}
</style>
