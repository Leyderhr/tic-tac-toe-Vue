<template>
  <div class="d-flex flex-column justify-content-between align-items-center vh-100 w-100">
    <!-- Encabezado -->
    <header class="py-4 text-center">
      <h1 class="display-4 text-primary">Tic-Tac-Toe Game!</h1>
    </header>

    <!-- Contenido principal (Juego) -->
    <main class="flex-grow-1 d-flex justify-content-center align-items-center">
      <div>
        <div class="board mx-auto">
          <div
            v-for="(cell, index) in board"
            :key="index"
            class="cell"
            :style="{ color: getCellColor(cell) }"
            @click="makeMove(index)"
          >
            {{ cell }}
          </div>
        </div>

        <!-- Mensaje de ganador -->
        <div v-if="winner" class="mt-3 text-success winner-message">Winner: {{ winner }}!</div>

        <!-- Contenedor para el botón y los indicadores -->
        <div class="d-flex justify-content-center align-items-center mt-4">
          <button class="btn btn-primary" @click="resetGame">Reiniciar Juego</button>

          <!-- Indicadores de turno -->
          <div class="d-flex ms-3">
            <div
              class="turn-indicator Xindicator me-2"
              :class="{ 'active-turn': currentPlayer === 'X' }"
            >
              X
            </div>
            <div
              class="turn-indicator Oindicator"
              :class="{ 'active-turn': currentPlayer === 'O' }"
            >
              O
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Footer -->
    <footer class="py-3 bg-none text-light text-center w-100">
      <div>
        <a
          href="mailto:leyderhr5@gmail.com.com"
          target="_blank"
          class="text-light mx-2"
          title="Enviar email"
        >
          <img src="https://img.icons8.com/fluency/48/gmail-new.png" alt="Email" />
        </a>
        <a
          href="https://www.linkedin.com/in/leyder-ignacio-47404a222"
          target="_blank"
          class="text-light mx-2"
          title="LinkedIn"
        >
          <img src="https://img.icons8.com/fluency/48/linkedin.png" alt="LinkedIn" />
        </a>
        <a
          href="https://github.com/Leyderhr"
          target="_blank"
          class="text-light mx-2"
          title="GitHub"
        >
          <img src="https://img.icons8.com/fluency-systems-filled/50/github.png" alt="GitHub" />
        </a>
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: ['', '', '', '', '', '', '', '', ''], // Tablero vacío
      currentPlayer: 'X', // Jugador inicial
      winner: null, // Sin ganador al inicio
      xColor: '#ff0000', // Color inicial para "X" (rojo)
      oColor: '#0000ff', // Color inicial para "O" (azul)
    }
  },
  methods: {
    makeMove(index) {
      if (this.board[index] === '' && !this.winner) {
        this.board[index] = this.currentPlayer
        if (this.checkWinner()) {
          this.winner = this.currentPlayer
          this.showWinnerMessage()

          setTimeout(() => {
            this.resetGame()
          }, 3000) // 3000 milisegundos = 3 segundos
        } else {
          this.currentPlayer = this.currentPlayer === 'X' ? 'O' : 'X'
        }
      }
    },
    checkWinner() {
      const winningCombinations = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ]
      return winningCombinations.some((combination) =>
        combination.every((index) => this.board[index] === this.currentPlayer),
      )
    },
    showWinnerMessage() {
      setTimeout(() => {
        this.winner = null // Esconde el mensaje después de un segundo
      }, 3000)
    },
    resetGame() {
      this.board = ['', '', '', '', '', '', '', '', '']
      this.currentPlayer = 'X'
      this.winner = null
    },
    getCellColor(cell) {
      if (cell === 'X') return this.xColor
      if (cell === 'O') return this.oColor
      return 'black' // Color predeterminado para celdas vacías
    },
  },
}
</script>

<style scoped>
/* Estilos del tablero */

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px); /* 3 columnas de 100px */
  grid-template-rows: repeat(3, 100px); /* 3 filas de 100px */
  gap: 5px; /* Espacio entre celdas */
  justify-content: center;
  margin: 0 auto;
}

.cell {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid #1e62a7; /* Borde gris */
  font-size: 2rem; /* Tamaño del texto */
  font-weight: bold;
  cursor: pointer;
  background-color: #57a5d1; /* Fondo claro */
}

.cell:hover {
  background-color: #f1a261; /* Fondo al pasar el mouse */
}

.cell:active {
  background-color: #ff8c42; /* Fondo al hacer clic */
}

/* Indicadores de turno */
.turn-indicator {
  width: 50px;
  height: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2rem;
  font-weight: bold;
  background-color: none;
  border: none;
}

.Xindicator {
  color: red;
}

.Oindicator {
  color: blue;
}
.turn-indicator.active-turn {
  background-color: #f0ad4e; /* Color de fondo cuando está activo */
  border-color: #f0ad4e; /* Borde de color cuando está activo */
}

/* Estilos del mensaje de ganador */
.winner-message {
  font-size: 3rem; /* Tamaño grande del texto */
  color: #28a745; /* Color verde para el mensaje */
  font-weight: bold;
  text-transform: uppercase;
}
</style>

<style>
/* Estilos globales */
body {
  background-color: #f0f5f1;
  margin: 0;
  font-family: Arial, sans-serif; /* Fuente general */
}

/* Footer */
footer a {
  text-decoration: none;
}

footer a:hover {
  text-decoration: underline;
}
</style>
