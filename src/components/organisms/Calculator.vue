<script setup lang="ts">
import { computed, ref } from "vue";

const expression = ref("");

const expressionFormatted = computed(() => {
  try {
    let format: string = expression.value;

    format = format.replace(/\*\*/g, "^");
    format = format.replace(/\//g, "<hr/>");

    format = format.replace(
      /\^(\d+)(?=[^\d]|$)/g,
      (_: string, exponent: string) => {
        return `<sup>${exponent}</sup>`;
      }
    );
    format = format.replace(/\/(\d)/g, (_: string, exponent: string) => {
      return `<hr /> ${exponent}`;
    });

    format = format.replace(/\+/g, " + ");
    format = format.replace(/\-/g, "  -  ");
    format = format.replace(/\*/g, " x ");
    format = format.replace(/\%/g, " % ");

    return format;
  } catch {
    return "pedro";
  }
});

const expressionCalc = computed(() => {
  try {
    return eval(expression.value);
  } catch {
    return null;
  }
});
</script>

<template>
  <div id="container">
    <div class="expression">
      <span v-html="expressionFormatted"></span>

      <span class="result" v-if="expressionCalc || expressionCalc == 0">
        <span> = </span>
        <span>{{ expressionCalc }}</span>
      </span>
    </div>

    <div class="input">
      <v-text-field
        class="w-100 h-100 bg-white text-black rounded"
        hide-details="auto"
        label="Expression"
        v-model="expression"
      ></v-text-field>
    </div>
  </div>
</template>

<style lang="scss" scoped>
#container {
  min-width: 10rem;

  display: flex;
  flex-direction: column;
  gap: 2rem;

  .expression {
    display: flex;
    align-items: center;
    gap: 2rem;

    & > span {
      text-align: center;
      font-size: 1.5rem;

      &.result {
        display: flex;
        gap: 2rem;
      }
    }
  }

  .input {
    width: 100%;
  }
}
</style>
