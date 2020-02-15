<template>
  <div class="sudoku">
    <h2>Sudoku</h2>
    <div class="grid">
      <div class="row" v-for="(row, rowIndex) in puzzle" v-bind:key="rowIndex">
        <div
          class="cell"
          v-for="(cell, colIndex) in row"
          :key="colIndex"
          v-bind:class="{'border-right': colIndex===2||colIndex===5,'border-bottom': rowIndex===2||rowIndex===5}"
        >{{ cell }}</div>
      </div>
    </div>
  </div>
</template>

<script>
// import sudoku package to generate a puzzle
import { sudoku } from "sudoku.js/sudoku.js";

export default {
  name: "Sudoku",
  data() {
    return {
      puzzle: [],
      difficulty: "easy"
    };
  },
  mounted() {
    this.generatePuzzle();
  },
  methods: {
    generatePuzzle() {
      //build-in method in the sudoku.js
      const boardString = sudoku.generate(this.difficulty);
      //build-in method in the sudoku.js
      this.puzzle = sudoku.board_string_to_grid(boardString);
      console.log("puzzle:", this.puzzle);
    }
  }
};
</script>

<style scoped>
.sudoku {
  width: calc(9 * 40px);
  margin: 0.5rem auto;
  font-family: Arial, Helvetica, sans-serif;
}
.row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.cell {
  display: block;
  width: 40px;
  height: 40px;
  /* border size included in the widht: */
  box-sizing: border-box;
  border: 1px solid #bbb;
  font-size: 24px;
  line-height: 40px;
  text-align: center;
  cursor: default;
}
.cell.border-right {
  border-right-width: 3px;
  border-right-color: rgb(139, 139, 139);
}
.cell.border-bottom {
  border-bottom-width: 3px;
  border-bottom-color: rgb(139, 139, 139);
}
</style>
