<script setup lang="ts">
import {  RouterView } from 'vue-router'
import NavBar from './components/Navigation/NavBar.vue';
import Footer from './components/Navigation/Footer.vue';
</script>

<template>
  <div>
 <NavBar class="mb-4"/>
    <RouterView />
    <Footer/>
  </div>
</template>

