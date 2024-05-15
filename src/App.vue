<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const checkRowForSame = (row) => {
  if (row.every((cell) => cell === row[0] && cell !== "")) {
    return true;
  }

  return false;
};

const threeInRowHorizontally = computed(() => {
  for (let i = 0; i < board.value.length; i++) {
    const element = board.value[i];
    if (checkRowForSame(element)) return true;
  }

  return false;
});

const isWin = computed(() => {
  return threeInRowHorizontally
});

const move = (x, y) => {
  if (board.value[x][y] !== "") return;
  board.value[x][y] = player.value;
  //player.value === "X" ? (player.value = "0") : (player.value = "X");
  if (!isWin.value.value) {
    if (player.value === "X") {
      player.value = "0";
    } else {
      player.value = "X";
    }
  }
};
</script>

<template>
  <main class="text-center">
  dsfsd{{ threeInRowHorizontally }}sdfsdf
    <h1 class="mt-16 text-3xl font-bold">Tic-Tac-Toe</h1>
    <h3 v-if="!isWin" class="text-xl mt-3">Player {{ player }}'s turn</h3>
    <h3 v-else>Player {{ player }} wins</h3>

    <div class="pt-4 flex flex-col justify-center align-center items-center">
      <div v-for="(row, x) in board" class="flex">
        <button
          v-for="(cell, y) in row"
          class="text-3xl w-24 h-24 border border-white"
          :class="{
            'text-green-600': board[x][y] === 'X',
            'text-orange-500': board[x][y] === '0',
          }"
          @click="move(x, y)"
        >
          {{ cell }}
        </button>
      </div>
    </div>
  </main>
</template>
