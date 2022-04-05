<script>
import "./assets/base.css";

const wins = [
  // horizontal
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
  // vertical
  [0, 3, 6],
  [1, 4, 7],
  [2, 5, 8],
  // diagonal
  [0, 4, 8],
  [2, 4, 6],
];

const Grid = () => ({
  owner: null,
});
const buildGrid = () =>
  Array(9)
    .fill()
    .map(() => Grid());

export default {
  data() {
    return {
      shake: false,
      grid: buildGrid(),
      players: [
        { id: "p1", name: "Player 1", symbol: "O" },
        { id: "p2", name: "Player 2", symbol: "X" },
      ],
      currentPlayerIdx: 0,
      winner: false,
    };
  },
  computed: {
    currentPlayer() {
      return this.players[this.currentPlayerIdx];
    },
  },
  methods: {
    checkForWin(player) {
      const cells = this.grid.filter((cell) => cell.owner === player);

      return wins.some((winCondition) =>
        winCondition.every((idx) => cells.includes(idx))
      );
    },
    cellClick(cell, idx) {
      if (cell.owner === null) {
        this.grid[idx].owner = this.currentPlayerIdx;
        this.nextPlayer();
      }
    },
    nextPlayer() {
      this.currentPlayerIdx = 1 - this.currentPlayerIdx;
    },
    changeName() {
      const name = window.prompt("Enter new name:");
      if (name) this.currentPlayer.name = name;
    },
  },
};
</script>

<template>
  <div id="tictactoe">
    <h1>Tic Tac Toe</h1>
    <h2>
      Current Player's Turn: {{ currentPlayer.name }}
      <button @click="changeName">Edit</button>
    </h2>
    <div id="board">
      <div
        v-for="(cell, idx) in grid"
        :key="idx"
        @click="cellClick(cell, idx)"
        :data-owner="cell.owner"
      >
        {{ players[cell.owner]?.symbol }}
      </div>
    </div>
  </div>
</template>
