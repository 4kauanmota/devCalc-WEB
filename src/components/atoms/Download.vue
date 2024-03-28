<script setup lang="ts">
import html2canvas from "html2canvas";

const downloadImage = () => {
  const table = document.getElementById("download");

  console.log(table);

  html2canvas(table as any).then(function (canvas) {
    const link = document.createElement("a");
    link.download = "Dev_Calc.png";
    link.href = canvas.toDataURL("image/png");
    link.click();
  });
};
</script>

<template>
  <div>
    <div id="download">
      <slot></slot>
    </div>

    <!-- Button to trigger the download -->
    <button
      @click="downloadImage"
      class="w-100 pt-4 text-button font-weight-bold"
    >
      Download
    </button>
  </div>
</template>

<style lang="scss" scoped>
button {
  font-size: 1rem !important;

  &:hover {
    text-decoration: underline;
  }
}
</style>
