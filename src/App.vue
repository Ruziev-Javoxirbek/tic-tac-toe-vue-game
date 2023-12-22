<script setup lang="ts">
import { ref } from 'vue';

const cells = ref(['', '', '', '', '', '', '', '', '']);
const currentPlayer = ref('X');
let gameResult = '';

const makeMove = (index: number) => {
  if (!gameResult && cells.value[index] === '') {
    cells.value[index] = currentPlayer.value;
    updateGameResult();
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
  }
};

const isVictory = () => {
  const combs = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
    [0, 1, 2],
    [3, 4, 5],
  ];
  
  for (const comb of combs) {
    if (
      cells.value[comb[0]] === cells.value[comb[1]] &&
      cells.value[comb[1]] === cells.value[comb[2]] &&
      cells.value[comb[0]] !== ''
    ) {
      return true;
    }
  }

  return false;
};

const updateGameResult = () => {
  if (isVictory()) {
    gameResult = 'Winner ' + currentPlayer.value;
  } else if (!cells.value.includes('')) {
    gameResult = 'draw:)';
  } else {
    gameResult = '';
  }
};

const gameAgain = () => {
  cells.value = ['', '', '', '', '', '', '', '', ''];
  currentPlayer.value = 'X';
  gameResult = '';
};

</script>

<template>
  <h1>tic tac toe game</h1>
  <h2>click on the square to make a move!</h2>
   <table class="game-field">
        <tr>
          <td v-for="(cell, index) in cells" @click="makeMove(index)" v-text="cell"></td>
        </tr>
      </table>
      <div class="result" v-text="gameResult"></div>
      <button class="again" @click="gameAgain()">New game</button>
</template>

<style scoped>
.game-field {
   margin: 0 auto;
}
.game-field tr {
  display: grid;
  grid-template-columns: auto auto auto ;
  grid-template-rows: auto auto auto ;
}
.game-field td {
  background-color: #4618ef;
  border: 1px solid #333333;
  color: #000000;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 20px;
  font-size: 25px;
  margin: 2px;
  cursor: pointer;
} 

h1 {
  font-size: 36px;
}
.again {
  border: none;
  border-radius: 16px;
  color: #3e1c1c;
  margin-top: 32px;
  transition: ease-out 0.3s;
  background-color: #987878;
}
.again:focus {
  outline: none;
}
.again:focus, 
.again:hover {
  transform: scale(2.05);
  background: #f57272;
}
.result{
  font-size: 25px;
  animation: example 4s infinite;
}

@keyframes example {
  0% { transform: scale(1); }
  50% { transform: scale(1.2); }
  70% { transform: rotate(30deg); }
  100% { transform: skew(20deg, 10deg);}
}
</style>
