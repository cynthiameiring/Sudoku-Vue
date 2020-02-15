<template>
  <div class="sudoku">
    <h2>Sudoku</h2>
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
      const boardString = sudoku.generate(this.difficulty);
      //build-in method in the sudoku.js
      this.puzzle = sudoku.board_string_to_grid(boardString);
      console.log(this.puzzle);
    }
  }
};
</script>

<style scoped>
.sudoku {
  width: 400px;
  margin: 0.5rem auto;
  font-family: Arial, Helvetica, sans-serif;
}
</style>
