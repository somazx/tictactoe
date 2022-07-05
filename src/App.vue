<script>
import "./assets/base.css";

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
      count: 0,
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
    gameWinState() {
      const gMap = { 0: "O", 1: "X", null: "-" };
      const gridString = this.grid.map((cell) => gMap[cell.owner]).join("");
      return (
        gridString.match(/X..X..X|X.X.X|XXX/) ||
        gridString.match(/O..O..O|O.O.O|OOO/)
      );
    },
  },
  methods: {
    cellClick(cell) {
      if (cell.owner === null) {
        cell.owner = this.currentPlayerIdx;
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
    newGame() {
      this.grid = buildGrid();
      this.currentPlayerIdx = 0;
    },
  },
};
</script>

<template>
  <div id="tictactoe">
    <h1 @click="count = count + 1">Tic Tac Toe {{ count }}</h1>
    <button type="button" @click="newGame()">Restart Game</button>Board Status:
    {{ gameWinState }}.
    <h2>
      Current Player's Turn: {{ currentPlayer.name }}
      <button @click="changeName">Edit</button>
    </h2>
    <div v-if="gameWinState">GAME OVER?!</div>
    <div id="board">
      <div
        v-for="(cell, idx) in grid"
        :key="idx"
        @click="cellClick(cell)"
        :data-owner="cell.owner"
      >
        {{ players[cell.owner]?.symbol }}
      </div>
    </div>
  </div>
</template>
