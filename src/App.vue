<script setup>
import { ref } from 'vue'
const winnerIndices = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  [0, 4, 8],
  [2, 4, 6]
]
const squares = ref(Array(9).fill(null))
const currentPlayer = ref('X')
const scores = ref({ X: 0, O: 0 })

const checkWinner = () => {
  for (const [a, b, c] of winnerIndices) {
    if (squares.value[a] && squares.value[a] === squares.value[b] && squares.value[a] === squares.value[c]) {
      return squares.value[a]
    }
  }
  return null
}

const isDraw = () => {
  return squares.value.every(square => square !== null) && !checkWinner()
}

const resetGame = () => {
  squares.value = Array(9).fill(null)
  currentPlayer.value = 'X'
}

const handleClick = (index) => {
  if (squares.value[index] || checkWinner()) return
  squares.value[index] = currentPlayer.value
  if (checkWinner()) {
    scores.value[currentPlayer.value]++
  }
  currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
}
</script>

<template>
 <div class="flex flex-col items-center mt-16">
  <div class="text-5xl mb-8 font-bold text-blue-600">Tic Tac Toe</div>
  <div class="flex justify-between w-full max-w-md mb-8">
    <div class="text-3xl text-blue-500">Player X: {{ scores.X }}</div>
    <div class="text-3xl text-blue-500">Player O: {{ scores.O }}</div>
  </div>
  <div v-if="!checkWinner() && !isDraw()" class="flex justify-center items-center text-5xl">
    <div class="grid grid-cols-3 gap-2">
      <div v-for="(square, index) in squares" :key="index" class="bg-gray-200 border border-black w-30 h-30 flex justify-center items-center text-5xl transition-transform duration-500 ease-in-out transform hover:scale-110" @click="handleClick(index)">
        {{ square }}
      </div>
    </div>
  </div>
  <div v-else class="flex flex-col items-center text-5xl">
    <div v-if="checkWinner() === 'X'" class="text-green-500 animate-bounce">Player X wins!</div>
    <div v-else-if="checkWinner() === 'O'" class="text-green-500 animate-bounce">Player O wins!</div>
    <div v-else class="text-red-500">It's a draw!</div>
    <button @click="resetGame" class="mt-8 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-700 transition-colors duration-300">Restart</button>
  </div>
</div>
</template>

<style scoped>
.w-30 {
  width: 120px;
  height: 120px;
}
</style>