<template>
  <div class="block-flex">
    <div>
      <div v-for="(row, rowIndex) in grid" :key="'row' + rowIndex" class="row flex">
        <div
          v-for="(cell, cellIndex) in row"
          :key="'cell' + cellIndex"
          class="cell w-[50px] h-[50px] border-[1px] border-[black]"
          :class="cell ? 'bg-black' : 'bg-white'"
          @click="updateCell(rowIndex, cellIndex)"
        ></div>
      </div>
    </div>
    <div class="flex flex-col">
      <div>
        <p>Etape : {{ step }}</p>
      </div>
      <div @click="stepPlus">
        <button class="btn">Etape suivante</button>
      </div>
      <div>
    <button class="btn" @click="run" v-if="!intervalId">Lancer</button>
    <button class="btn" @click="stop" v-else>Stop</button>
  </div>
      <div @click="clear">
        <button class="btn">Clear</button>
    </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      grid: Array(14)
        .fill()
        .map(() => Array(14).fill(false)),
      trueCells: [],
      step: 0,
    }
  },

  mounted() {},

  methods: {
  // modifie la valeur de la cellule true ou false
    updateCell(rowIndex, cellIndex) {
      this.grid[rowIndex][cellIndex] = !this.grid[rowIndex][cellIndex]

      if (this.grid[rowIndex][cellIndex]) {
        this.trueCells.push({ row: rowIndex, cell: cellIndex });
      } 
    },

    stepPlus() {
      // tabbleau qui contiendra les nouvelles valeurs des cellules
      let newStates = [];
      let step = this.step++;
      // parcours de chaque cellule de la grille 
      for (let i = 0; i < this.grid.length; i++) {
        for (let j = 0; j < this.grid[i].length; j++) {

          let adjacentTrueCount = this.countAdjacentTrue(i, j);

          if (!this.grid[i][j] && adjacentTrueCount === 3) {
            newStates.push({ row: i, cell: j, state: true })
          } else if (this.grid[i][j] && (adjacentTrueCount < 2 || adjacentTrueCount > 3)) {
            newStates.push({ row: i, cell: j, state: false })
          }
        }
      }

      newStates.forEach(({ row, cell, state }) => {
        this.grid[row][cell] = state
      })
    },

    countAdjacentTrue(row, cell) {
      let count = 0;
      // possibilités de cellules adjacentes
      const possibilities = [
        [row - 1, cell - 1],
        [row, cell - 1],
        [row + 1, cell - 1],
        [row - 1, cell],
        [row + 1, cell],
        [row - 1, cell + 1],
        [row, cell + 1],
        [row + 1, cell + 1]
      ];


      possibilities.forEach(([x, y]) => {
        if (
          x >= 0 &&
          x < this.grid.length &&
          y >= 0 &&
          y < this.grid[0].length &&
          this.grid[x][y]
        ) {
          count++
        }
      })

      return count
    },

    run(){
      this.intervalId = setInterval(() => {
      this.stepPlus();
    }, 500);
    if(!this.grid[rowIndex][cellIndex])
    {
      this.stop();
    }
    },

    stop(){
      if (this.intervalId !== null) {
        clearInterval(this.intervalId);
        this.intervalId = null;
      } 
    },
    clear(){
      let step = 0;
      this.grid = Array(14)
        .fill()
        .map(() => Array(14).fill(false));
    }
  }
}
</script>
