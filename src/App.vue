<template>
  <div class="grid-container">
    <div></div>
      <input
        type="number"
        placeholder="Enter number of rows and columns"
        min="2"
        max="25"
        v-model="rowCount"
        @keydown="onKeyDown"
      />
    <grid :row-count="gridOrder" />
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import Grid from "./components/Grid.vue";

const rowCount = ref<number>(4);
const gridOrder = ref<number>(4);

const onKeyDown = (event: KeyboardEvent) => {
  if (event.key !== "Enter") return;

  if (rowCount.value < 4) {
    alert("Input must be greater than or equal to 4");
    return;
  } else if (rowCount.value > 25) {
    alert("Input must be greater less or equal to 25");
    return;
  }

  gridOrder.value = rowCount.value
};
</script>

<style scoped>
input {
  display: block;
  padding: 10px 15px;
  width: 100%;
}

input::placeholder {
  font-size: 16px;
}

.grid-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  gap: 15px;
  height: 100vh;
}

@media screen and (max-width: 768px) {
  input {
    padding: 10px;
  }

  input::placeholder {
    font-size: 14px;
  }
}
</style>
