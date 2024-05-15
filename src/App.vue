<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const move = (x, y) => {
  if (board.value[x][y] !== "" || isOver.value) return;
  board.value[x][y] = player.value;
  if (!winner.value)
    player.value === "X" ? (player.value = "0") : (player.value = "X");
};

const areTheSame = (row) => {
  return row.every((cell) => cell !== "" && cell === row[0]);
};

const isHorizontalWin = computed(() => {
  for (let i = 0; i < board.value.length; i++) {
    if (areTheSame(board.value[i])) return board.value[i][0];
  }

  return false;
});

const isVerticalWin = computed(() => {
  const columns = board.value.reduce(
    (result, row) => row.map((cell, i) => (result[i] || []).concat(cell)),
    []
  );
  for (let i = 0; i < columns.length; i++)
    if (areTheSame(columns[i])) return columns[i][0];

  return false;
});

const isDiagonalWin = computed(() => {
  if (areTheSame([board.value[0][0], board.value[1][1], board.value[2][2]])) {
    return board.value[0][0];
  }

  if (areTheSame([board.value[0][2], board.value[1][1], board.value[2][0]])) {
    return board.value[0][0];
  }

  return false;
});

const reset = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];

  player.value = "X";
};

const winner = computed(
  () => isHorizontalWin.value || isVerticalWin.value || isDiagonalWin.value
);

const isTie = computed(() => !winner.value && !board.value.flat().includes(""));

const isOver = computed(() => winner.value || isTie.value);
</script>

<template>
  <main class="flex flex-col justify-center items-center mt-16">
    <h3 class="text-3xl font-bold mb-4">Tic-Tac-Toe</h3>
    <h3 v-if="!isOver" class="text-xl mb-4">Player {{ player }} turn</h3>
    <div v-else class="mb-4 text-center">
      <h3 v-if="winner" class="font-bold text-xl">Player {{ winner }} wins!</h3>
      <h3 v-else class="font-bold text-xl">It's a tie</h3>
      <button
        class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded mt-4"
        v-on:click="reset"
      >
        New Game
      </button>
    </div>

    <div v-for="(row, x) in board" class="flex text-3xl">
      <button
        v-for="(cell, y) in row"
        class="w-24 h-24 border border-white"
        :class="{
          'text-green-600': cell === 'X',
          'text-orange-500': cell === '0',
        }"
        v-on:click="move(x, y)"
      >
        {{ cell }}
      </button>
    </div>
  </main>
</template>
