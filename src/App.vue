<script setup>
import { ref, computed } from "vue";
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);
const CalculateWinner = (board) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }
  return null;
};
const winner = computed(() => CalculateWinner(board.value.flat()));
const MakeMove = (x, y) => {
  if (winner.value) return;
  if (board.value[x][y]) return;
  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};
const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<template>
  <img
    src="./assets/tictactoe.png"
    alt=""
    class="object-scale-down h-15 w-20 mt-2"
  />
  <main class="pt-8 text-center">
    <h1 class="mb-8 text-3xl text-pink-600 font-bold uppercase">Tic Tac Toe</h1>

    <h3 class="text-xl mb-4">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`border border-white w-24 h-24 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
        >
          {{ cell === "X" ? "close" : cell === "O" ? "circle" : "" }}
        </div>
      </div>
    </div>

    <div class="text-center">
      <h2 v-if="winner" class="text-6xl font-bold mb-8">
        Player '{{ winner }}' wins!
      </h2>
      <button
      @click="ResetGame"
        class="
          bg-pink-600
          hover:bg-pink-400
          text-white
          font-bold
          py-2
          px-4
          border-b-4 border-pink-800
          hover:border-pink-500
          rounded
          uppercase
          duration-200
        "
      >
        Reset Game
      </button>
    </div>
  </main>
</template>

<style>
body {
  @apply bg-gray-800 text-white;
}
</style>
