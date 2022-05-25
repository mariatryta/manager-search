<template>
  <header>
    <h1>Find the manager</h1>
  </header>

  <RouterView />
</template>

<script setup>
import { RouterView } from "vue-router";
</script>

<style>
@import "../node_modules/modern-normalize/modern-normalize.css";
@import "@/assets/base.css";

#app {
  display: flex;
  flex-direction: column;
  max-width: 1280px;
  margin: 0 auto;
  font-weight: normal;
  min-height: 100vh;
  width: 100%;
}

header {
  line-height: 1.5;
  padding: 2rem;
}

#main {
  padding: 0 2rem;
}
</style>
