<script setup lang="ts">
import { computed, ref } from "vue";

const expression = ref("");

const cleanExpression = computed(() => {
  expression.value = expression.value.replace(/[a-zA-Z]/g, "");

  expression.value = expression.value.replace(/=/g, "");

  expression.value = expression.value.replace(
    /\+\-|\-\+|\+\*|\*\+|\+\%|\%\+|\+\/|\/\+|\-\-|\-\*|\-\%|\%\-|\*\/|\/\*|\%\*|\/\%/g,
    (match: string) => match[1]
  );

  expression.value = expression.value.replace(
    /(\+{2}|-{2}|\*\*{2}|%{2}|\/{2})/g,
    (match: string) => match[0]
  );
});

const expressionFormatted = computed(() => {
  let format: string = expression.value;

  format = format.replace(/\*\*/g, "^");

  format = format.replace(
    /(\d+)(\^\d+)?\/(\d+)/g,
    (match: string, _: string) => {
      const division = match.split("/");
      return `<span>${division[0]} <span style="display: block;"><hr/>${division[1]}</span></span>`;
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

  format = format.replace(/\+/g, "<i>+</i>");
  format = format.replace(/\-/g, "<i>-</i>");
  format = format.replace(/\*/g, "<i>x</i>");
  format = format.replace(/\%/g, "<i>%</i>");

  return format;
});

const expressionCalc = computed(() => {
  try {
    return eval(expression.value);
  } catch (error) {
    console.log(error);
    return null;
  }
});
</script>

<template>
  <div id="container" class="d-flex flex-column ga-4">
    <div
      class="expression w-100 d-flex justify-center align-center ga-2 overflow-x-auto"
    >
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
        class="bg-white text-black rounded"
        hide-details="auto"
        label="Expression"
        v-model="expression"
        @input="cleanExpression"
      ></v-text-field>
    </div>
  </div>
</template>

<style lang="scss" scoped>
@use "/src/theme/screens.scss";

#container {
  min-width: 15rem;
  max-width: 80%;

  .expression > span {
    text-align: center;
    font-size: 1.5rem;
  }
}

@media (max-width: screens.$mobile-max) {
  #container {
    min-width: 80%;
  }
}
</style>
