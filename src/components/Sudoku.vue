<template>
  <div class="sudoku">
    <h2>Sudoku</h2>
    <div class="grid">
      <div class="row" v-for="(row, rowIndex) in puzzle" v-bind:key="rowIndex">
        <div
          class="cell"
          v-for="(cell, colIndex) in row"
          @click="setCellActive(rowIndex, colIndex, cell.startValue)"
          v-bind:key="colIndex"
          v-bind:class="{
            'border-right': colIndex===2||colIndex===5,
            'border-bottom': rowIndex===2||rowIndex===5, 
            'startValue':cell.startValue,
            'active':activeRow===rowIndex&&activeCol===colIndex }"
        >{{ cell.value }}</div>
      </div>
    </div>
    <div class="row">
      <button
        type="button"
        class="btn"
        v-for="value in Array(9).keys()"
        :key="value"
        :disabled="activeRow===null || activeCol===null"
      >{{value+1}}</button>
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
      difficulty: "easy",
      activeRow: null,
      activeCol: null
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
      const boardGrid = sudoku.board_string_to_grid(boardString);

      this.puzzle = boardGrid.map(row =>
        row.map(cell => {
          return {
            value: cell !== "." ? parseInt(cell) : null,
            //startvalue is true or false
            startValue: cell !== "."
          };
        })
      );
      console.log("puzzle:", this.puzzle);
    },
    setCellActive(row, col, startValue) {
      if (startValue) {
        return;
      }
      if (this.activeRow === row && this.activeCol === col) {
        this.activeRow = null;
        this.activeCol = null;
        return;
      }
      this.activeRow = row;
      this.activeCol = col;
    }
  }
};
</script>

<style scoped>
.sudoku {
  width: 100%;
  max-width: 420px;
  margin: 0.5rem auto;
  font-family: Arial, Helvetica, sans-serif;
}

.grid {
  width: calc(9 * 40px);
  margin: 0.5rem auto 1rem;
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
.cell.startValue {
  font-weight: bold;
}
.cell:not(.startValue) {
  cursor: pointer;
}
.cell.active {
  background-color: blue;
  color: white;
}
.btn {
  width: 38px;
  height: 38px;
  font-size: 24px;
  cursor: pointer;
}
.btn:disabled {
  cursor: not-allowed;
}
</style>
