<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const checkRowForSame = (row) => {
  if (row.filter((i) => i !== "").length) {
    const threeInRow = row.every((i) => i === row[0]);
    if (threeInRow) return row[0];
  }

  return false;
};

const threeInRowHorizontally = computed(() => {
  for (let i = 0; i < board.value.length; i++) {
    if (checkRowForSame(board.value[i])) {
      return board.value[i][0];
    }
  }

  return false;
});

const threeInRowVertically = computed(() => {
  const columns = board.value.reduce(
    (result, row) =>
      row.map((value, index) => (result[index] || []).concat(value)),
    []
  );

  for (let i = 0; i < columns.length; i++) {
    if (checkRowForSame(columns[i])) {
      return columns[i][0];
    }
  }
  console.log(columns);
});

const threeInRowDiagonally = computed(() => {
  const leftToRight = checkRowForSame([
    board.value[0][0],
    board.value[1][1],
    board.value[2][2],
  ]);

  if (leftToRight) return leftToRight;

  const rightToLeft = checkRowForSame([
    board.value[0][2],
    board.value[1][1],
    board.value[2][0],
  ]);

  if (rightToLeft) return rightToLeft;

  return false;
});

const winner = computed(() => {
  return (
    threeInRowHorizontally.value ||
    threeInRowVertically.value ||
    threeInRowDiagonally.value ||
    null
  );
});

const move = (x, y) => {
  if (winner.value || board.value[x][y] !== "") return;
  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "0" : "X";
};

const isOver = computed(() => winner.value || isTie.value);

const isTie = computed(() => {
  console.log(board.value.flat);
  return !board.value.flat().includes("") && !winner.value;
});

const reset = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];

  player.value = "X";
};
</script>

<template>
  <main class="pt-16 text-center">
    <h1 class="mb-3 text-3xl font-bold">Tic-Tac-Toe</h1>
    <h3 v-if="!isOver" class="mb-4 text-xl">Player {{ player }}'s turn</h3>
    <div v-else>
      <h3 v-if="winner" class="mb-8 text-6xl font-bold">Player '{{ winner }}' wins!</h3>
      <h3 v-if="isTie" class="mb-8 text-6xl font-bold">Cat got it!</h3>
      <button
        @click="reset"
        class="inline-block p-3 px-2 mb-8 bg-blue-700 rounded"
      >
        New Game
      </button>
    </div>
    <div class="flex flex-col items-center">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <button
          v-for="(cell, y) in row"
          :key="y"
          class="w-24 h-24 text-5xl border border-white"
          :class="{
            'text-lime-600': cell === 'X',
            'text-orange-500': cell === '0',
          }"
          @click="move(x, y)"
        >
          {{ cell }}
        </button>
      </div>
    </div>
  </main>
</template>
