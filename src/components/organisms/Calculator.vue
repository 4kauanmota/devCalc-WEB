<script setup lang="ts">
import { computed, ref } from "vue";

const expression = ref("");

const expressionFormatted = computed(() => {
  let format: string = expression.value;

  format = format.replace(/\*\*/g, "^");

  format = format.replace(
    /(\d+)(\^\d+)?\/(\d+)/g,
    (match: string, _: string) => {
      const division = match.split("/");
      return `<span class="space">${division[0]} <span style="display: block;"><hr/>${division[1]}</span></span>`;
    }
  );
  format = format.replace(/(\d+)\//g, (_: string, exponent: string) => {
    return `<span>${exponent} <span style="display: block;"><hr/></span></span>`;
  });

  format = format.replace(
    /\^(\d+)(?=[^\d]|$)/g,
    (_: string, exponent: string) => {
      return `<sup>${exponent}</sup>`;
    }
  );

  format = format.replace(/\+/g, " + ");
  format = format.replace(/\-/g, "  -  ");
  format = format.replace(/\*/g, " x ");
  format = format.replace(/\%/g, " % ");

  return format;
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
  <div id="container" class="d-flex flex-column ga-4">
    <div class="expression d-flex align-center ga-2">
      <span
        class="formatted d-flex align-center"
        v-html="expressionFormatted"
      ></span>

      <span
        class="result d-flex ga-3"
        v-if="expressionCalc || expressionCalc == 0"
      >
        <span> = </span>
        <span>{{ expressionCalc }}</span>
      </span>
    </div>

    <div class="w-100">
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

  .expression > span {
    text-align: center;
    font-size: 1.5rem;
  }
}
</style>
