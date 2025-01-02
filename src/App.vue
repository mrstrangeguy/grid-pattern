<template>
  <div class="container">
    <input type="number" placeholder="Enter number of rows and columns" v-model="rowCount" />
    <div class="grid-container">
      <grid :row-count="rowCount" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Grid from "./components/Grid.vue";

const rowCount = ref<number>(5);
</script>

<style scoped>
input {
  display: block;
  padding: 15px;
  width: 40%;
  margin-bottom: 20px;
}
input::placeholder {
  font-size: 16px;
}
/* .grid-container {
  height: 90vh;
} */
</style>
