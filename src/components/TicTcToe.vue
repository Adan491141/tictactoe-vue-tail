<template>
    <div class="min-h-screen bg-gray-900 flex items-center justify-center p-4">
      <div class="bg-gray-800 p-8 rounded-xl shadow-2xl max-w-md w-full">
        <h1 class="text-4xl font-bold text-center mb-8 text-white">Tic Tac Toe</h1>
        
        <!-- Tablro -->
        <div class="grid grid-cols-3 gap-3 mb-8">
          <button 
            v-for="(cell, index) in board" 
            :key="index"
            @click="makeMove(index)"
            :disabled="cell !== null || gameOver"
            class="w-full h-24 bg-gray-700 hover:bg-gray-600 disabled:hover:bg-gray-700 rounded-lg text-4xl font-bold text-white flex items-center justify-center transition-all duration-300"
          >
            <span :class="cell === 'X' ? 'text-blue-400' : 'text-red-400'">{{ cell }}</span>
          </button>
        </div>
        
        <!-- Estado del juego -->
        <div class="text-center mb-6">
          <p class="text-2xl font-semibold text-white">
            <span :class="{
              'text-green-400': winner,
              'text-blue-400': !winner && !gameOver,
              'text-yellow-400': !winner && gameOver
            }">
              {{ statusMessage }}
            </span>
          </p>
        </div>
        
        <!-- Boton Reiniciar -->
        <div class="flex justify-center">
          <button 
            @click="resetGame"
            class="px-6 py-3 bg-blue-500 hover:bg-blue-600 text-white font-semibold rounded-lg transition-all duration-300 transform hover:scale-105"
          >
            Reiniciar
          </button>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, computed } from 'vue'
  
  const board = ref(Array(9).fill(null))
  const xIsNext = ref(true)
  const winner = ref(null)
  const gameOver = ref(false)
  
  const statusMessage = computed(() => {
    if (winner.value) {
      return `Ganador: ${winner.value}`
    } else if (gameOver.value) {
      return "Empate!"
    } else {
      return `Turno de: ${xIsNext.value ? 'X' : 'O'}`
    }
  })
  
  const winningCombinations = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // Fils
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columnas
    [0, 4, 8], [2, 4, 6] // Diagonales
  ]
  
  function makeMove(index) {
    if (board.value[index] || winner.value) return
  
    board.value[index] = xIsNext.value ? 'X' : 'O'
    xIsNext.value = !xIsNext.value
  
    checkWinner()
    checkGameOver()
  }
  
  function checkWinner() {
    for (let combo of winningCombinations) {
      const [a, b, c] = combo
      if (
        board.value[a] &&
        board.value[a] === board.value[b] &&
        board.value[a] === board.value[c]
      ) {
        winner.value = board.value[a]
        return
      }
    }
  }
  
  function checkGameOver() {
    if (!board.value.includes(null)) {
      gameOver.value = true
    }
  }
  
  function resetGame() {
    board.value = Array(9).fill(null)
    xIsNext.value = true
    winner.value = null
    gameOver.value = false
  }
  </script>