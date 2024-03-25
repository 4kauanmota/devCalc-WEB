<script setup lang="ts">
  import { computed, ref } from 'vue';

  const expression = ref("");

  const expressionConverted = computed(() => {
    try{
      return eval(expression.value);
    }
    catch {
      return ""
    }
  })
</script>

<template>
  <div id="container">
    <span>{{ expressionConverted }}</span>
  
    <input type="text" v-model="expression" />
  </div>
</template>

<style lang="scss" scoped>
  #container {
    display: flex;
    flex-direction: column;
    gap: 4rem;
  }
</style>
