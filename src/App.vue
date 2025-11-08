<script setup lang="ts">
//import ChuseokPage from "./components/ChuseokPage.vue";
//import Prototype from "./components/Prototype.vue";
//import Composite from "./components/Composite.vue";
import TemplateMethod from "./components/TemplateMethod.vue";
</script>

<template>
  <TemplateMethod />
</template>

<style scoped>
:global(body, html) {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
  overflow-x: hidden;
  background: linear-gradient(to bottom, #0a0a23, #1a1a3d);
}

:global(#app) {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
