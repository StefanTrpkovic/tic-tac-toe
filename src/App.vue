<script setup>
import { ref, computed } from "vue"

const player = ref("X")
// const board = ref([
//   ["0", "1", "2"],
//   ["3", "4", "5"],
//   ["6", "7", "8"],
// ])

const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
])

const isHorizontalWin = computed(() => {
  for (let i = 0; i < board.value.length; i++) {
    const row = board.value[i]
    const zeroCell = row[0]

    if (row.every((cell) => cell === zeroCell)) {
      return zeroCell
    }
  }

  return false
})

const isVerticalWin = computed(() => {
  const firstColumn = [],
    secondColumn = [],
    thirdColumn = []

  for (let i = 0; i < board.value.length; i++) {
    const row = board.value[i]
    firstColumn.push(row[0])
    secondColumn.push(row[1])
    thirdColumn.push(row[2])
  }

  if (firstColumn.every((cell) => cell === firstColumn[0] && cell !== ""))
    return firstColumn[0]
  if (secondColumn.every((cell) => cell === secondColumn[0] && cell !== ""))
    return secondColumn[0]
  if (thirdColumn.every((cell) => cell === thirdColumn[0] && cell !== ""))
    return thirdColumn[0]

  return false
})

const isDiagonalWin = computed(() => {
  const firstDiagonal = [],
    secondDiagonal = []

  firstDiagonal.push(board.value[0][0], board.value[1][1], board.value[2][2])
  secondDiagonal.push(board.value[0][2], board.value[1][1], board.value[2][0])

  if (firstDiagonal.every((cell) => cell === firstDiagonal[0] && cell !== ""))
    return firstDiagonal[0]

  if (secondDiagonal.every((cell) => cell === secondDiagonal[0] && cell !== ""))
    return secondDiagonal[0]

  return false
})

const winner = computed(() => {
  return isHorizontalWin.value || isVerticalWin.value || isDiagonalWin.value
})

const isTie = computed(() => {
  let result = true
  for (let i = 0; i < board.value.length; i++) {
    const row = board.value[i]
    for (let i = 0; i < row.length; i++) {
      const cell = row[i]
      if (cell === "") {
        result = false
        break
      }
    }
  }

  return result
})

const onCellClick = (x, y) => {
  if (player.value === "X") {
    board.value[x][y] = "X"
    player.value = "0"
  } else {
    board.value[x][y] = "0"
    player.value = "X"
  }
}

const reset = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ]
}
</script>

<template>
  <main class="h-screen flex flex-col justify-center text-center">
    <div class="mb-4">
      <h1 class="text-3xl font-bold">Tic Tac Toe</h1>
      <p v-if="winner">
        Player {{ winner === "X" ? 1 : winner === "0" ? 2 : "" }} wins
      </p>
      <p v-else-if="isTie">It's a tie</p>
      <button @click="reset">Start new game</button>
    </div>
    <div v-for="(row, x) in board">
      <button
        v-for="(cell, y) in row"
        class="border border-white p-10"
        @click="onCellClick(x, y)"
        :disabled="winner || isTie || cell !== ''"
      >
        {{ cell }}
      </button>
    </div>
  </main>
</template>
