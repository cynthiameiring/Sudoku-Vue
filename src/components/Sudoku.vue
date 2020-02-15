<template>
  <div class="sudoku">
    <div class="row">
      <h2>Sudoku</h2>
      <select v-model="difficulty" @change="generatePuzzle()">
        <option v-for="(display,level) in levels" :key="level" :value="level">{{display}}</option>
      </select>
    </div>
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
            //if the cell has a value then check if it is valid:
            'invalid': cell.value && isCellInvalid(rowIndex,colIndex,cell.value),
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
        @click="setCellValue(value+1)"
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
      activeCol: null,
      levels: {
        easy: "Easy",
        medium: "Medium",
        hard: "Hard",
        "very-hard": "Very Hard",
        insane: "Insane",
        inhuman: "Inhuman"
      }
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
    },
    setCellValue(value) {
      this.puzzle[this.activeRow][this.activeCol].value = value;
      this.activeRow = null;
      this.activeCol = null;
    },
    isCellInvalid(row, col, value) {
      for (let c = 0; c < 9; c++) {
        if (this.puzzle[row][c].value === value && c !== col) {
          return true;
        }
      }
      for (let r = 0; r < 9; r++) {
        if (this.puzzle[r][col].value === value && r !== row) {
          return true;
        }
      }
      const rowStart = Math.floor(row / 3) * 3;
      const colStart = Math.floor(col / 3) * 3;
      for (let r = rowStart; r < rowStart + 3; r++) {
        for (let c = colStart; c < colStart + 3; c++) {
          if (this.puzzle[r][c].value === value && !(r === row && c === col)) {
            return true;
          }
        }
      }
      return false;
    }
  }
};
</script>

<style>
@import "../style.css";
</style>
