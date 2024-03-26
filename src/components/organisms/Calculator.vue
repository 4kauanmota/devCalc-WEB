<script setup lang="ts">
import { computed, ref } from "vue";

const expression = ref("");

const expressionFormatted = computed(() => {
  try {
    let format: string = expression.value;

    format = format.replace(/\*\*/g, "^");
    format = format.replace(
      /\^(\d+)(?=[^\d]|$)/g,
      (_: string, exponent: string) => {
        console.log(exponent);
        return `<sup>${exponent}</sup>`;
      }
    );

    format = format.replace(/\/(\d)/g, (_: string, exponent: string) => {
      return `<hr>${exponent}`;
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

    <input type="text" v-model="expression" />
  </div>
</template>

<style lang="scss" scoped>
#container {
  display: flex;
  flex-direction: column;
  gap: 2rem;

  .expression {
    display: flex;
    align-items: center;
    gap: 2rem;

    & > span {
      text-align: center;

      &.result {
        display: flex;
        gap: 2rem;
      }
    }
  }
}
</style>
