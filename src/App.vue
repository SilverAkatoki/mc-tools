<script setup lang="ts">
import TextInput from "./components/TextInput.vue";
import ItemCalculator from "./components/ItemCalculator.vue";
</script>

<template>
  <main class="container">
    <text-input />
    <item-calculator />
  </main>
</template>

<style scoped>
</style>

<style>

* {
  margin: 0;
  padding: 0;
  user-select: none;
}

:root {
  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
  font-size: 16px;
  line-height: 24px;
  font-weight: 400;

  color: #0f0f0f;
  background-color: #f6f6f6;

  font-synthesis: none;
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-text-size-adjust: 100%;
}

.container {
  display: flex;
  flex-direction: column;
  justify-content: center;;
  gap: 50px;
  padding-top: 20px;
}

</style>