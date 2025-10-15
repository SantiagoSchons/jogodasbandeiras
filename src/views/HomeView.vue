<script setup>
import { RouterLink, RouterView } from 'vue-router'
import TheWelcome from '../components/TheWelcome.vue'
</script>

<template>
  <main>
    <img :src="'image.png'" alt="">
    <h1>
      Encontre a bandeira
    </h1>
    <p>
      Indique a bandeira que corresponde ao país indicado
    </p>
    <router-link to="flag-app">iniciar</router-link>
  </main>
</template>


<style>
main{
  text-align: center;
  & img{
    width: 100%;
    margin-bottom: 80vw;
  }
}
</style>