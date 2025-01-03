<template>
  <div
    class="container"
    :style="{
      display: 'grid',
      gridTemplateRows: `repeat(${rowCount},1fr)`,
      gridTemplateColumns: `repeat(${rowCount},1fr)`,
    }"
  >
    <button
      v-for="(_, index) in Array(rowCount * rowCount)"
      @dblclick="onSelectBox(index)"
      :class="[
        'box',
        {
          'box-color': isBoxSelected(index),
          'box-gray': isConcurrentBox(index),
        },
      ]"
      :disabled="isConcurrentBox(index) || isBoxSelected(index)"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from "vue";

const selectedBoxIndexes = ref<number[]>([]);
const selectedRows = ref<number[]>([]);
const selectedColumnBoxes = ref<number[]>([]);
const selectedDiagonalBoxes = ref<number[]>([]);

const props = defineProps<{
  rowCount: number;
}>();

watch(
  () => props.rowCount,
  () => {
    selectedBoxIndexes.value = [];
    selectedRows.value = [];
    selectedColumnBoxes.value = [];
    selectedDiagonalBoxes.value = [];
  }
);

const onSelectBox = (index: number) => {
  selectedBoxIndexes.value.push(index);
  selectConcurrentBoxes(Math.floor(index / props.rowCount), index);
};

const selectConcurrentBoxes = (rowNumber: number, columnBoxIndex: number) => {
  selectRow(rowNumber);
  selectColumnBoxes(columnBoxIndex);
  selectDiagonalBoxes(
    columnBoxIndex,
    columnBoxIndex % props.rowCount,
    props.rowCount + 1
  );
  selectDiagonalBoxes(
    columnBoxIndex,
    columnBoxIndex % props.rowCount,
    -(props.rowCount - 1)
  );
};

const isBoxSelected = (index: number) => {
  return selectedBoxIndexes.value.includes(index);
};

const isRowSelected = (rowNumber: number) => {
  return selectedRows.value.includes(rowNumber);
};

const isColumnSelected = (columnNumber: number) => {
  return selectedColumnBoxes.value.includes(columnNumber);
};

const isDiagonalBoxesSelected = (boxNumber: number) => {
  return selectedDiagonalBoxes.value.includes(boxNumber);
};

const selectRow = (rowNumber: number) => {
  if (!isRowSelected(rowNumber)) {
    selectedRows.value.push(rowNumber);
  }
};

const selectColumnBoxes = (columnBoxIndex: number) => {
  let currentDecrementIndex = columnBoxIndex;
  let currentIncrementIndex = columnBoxIndex;

  while (
    currentDecrementIndex >= 0 ||
    currentIncrementIndex <= props.rowCount * props.rowCount
  ) {
    currentDecrementIndex -= props.rowCount;
    currentIncrementIndex += props.rowCount;

    selectedColumnBoxes.value.push(
      currentDecrementIndex,
      currentIncrementIndex
    );
  }
};

const selectDiagonalBoxes = (
  currentBoxIndex: number,
  columnNo: number,
  step: number
) => {
  let currentIncrementIndex = currentBoxIndex;
  let currentDecrementIndex = currentBoxIndex;
  let columnIncrementNo = columnNo;
  let columnDecrementNo = columnNo;

  console.log("column decrement", columnDecrementNo);

  while (
    (currentDecrementIndex >= 0 ||
      currentIncrementIndex < props.rowCount * props.rowCount) &&
    (columnIncrementNo < props.rowCount - 1 || columnDecrementNo > 0)
  ) {
    if (columnIncrementNo < props.rowCount - 1) currentIncrementIndex += step;
    if (columnDecrementNo > 0) currentDecrementIndex -= step;
    columnIncrementNo++;
    columnDecrementNo--;

    selectedDiagonalBoxes.value.push(
      currentIncrementIndex,
      currentDecrementIndex
    );
  }
};

const isConcurrentBox = (index: number) => {
  return (
    (isRowSelected(Math.floor(index / props.rowCount)) ||
      isColumnSelected(index) ||
      isDiagonalBoxesSelected(index)) &&
    !isBoxSelected(index)
  );
};
</script>

<style lang="scss" scoped>
.box {
  display: block;
  border: none;
  background-color: lightblue;
  width: 100%;
  height: 100%;
  outline: 1px solid green;
  cursor: pointer;
}

.box-color {
  background-color: lightcoral;
}

.box-gray {
  background-color: gray;
}

.concurrent-box-color {
  background-color: aquamarine;
}

.container {
  height: 85vh;
  aspect-ratio: 1/1;
}

@media screen and (max-width: 768px) {
  .container {
    height: 80vh;
  }
}

@media screen and (max-width: 560px) {
  .container {
    height: 65vh;
  }
}

@media screen and (max-width: 480px) {
  .container {
    height: 40vh;
  }
}

@media screen and (max-width: 375px) {
  .container {
    height: 40vh;
  }
}
</style>