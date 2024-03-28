<script setup lang="ts">
import Calculator from "./components/organisms/Calculator.vue";
</script>

<template>
  <h1>{Dev}/Calc</h1>

  <div id="container">
    <Calculator />
  </div>
</template>

<style lang="scss" scoped>
h1 {
  position: fixed;
  left: 0;
  right: 0;
  text-align: center;

  font-size: 3rem;
  margin: 1rem 0;
}

#container {
  height: 100vh;

  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
