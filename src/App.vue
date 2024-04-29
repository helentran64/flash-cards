<script setup>
// import { RouterLink, RouterView } from 'vue-router'
import Logo from './components/Logo.vue'
</script>

<template>
  <header>
    <Logo/>
    <!-- <nav>
      <RouterLink to="/">Home</RouterLink>
    </nav> -->
  </header>

  <body>
    <RouterView/>
  </body>

</template>

<style scoped>
  header{
    border: 1px white;
  }
</style>
