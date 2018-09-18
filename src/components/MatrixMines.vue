<template>
  <table class="matrix-mines">
    <tbody>
      <tr v-for="rowIndex in 8" :key="rowIndex">
        <MatrixElement 
            v-for="columnIndex in 8" :key="columnIndex" 
            :row-index=(rowIndex-1) 
            :column-index=(columnIndex-1) 
            :weight=matrixMines[rowIndex-1][columnIndex-1]
            :is-opened=isOpenMatrixMines[rowIndex-1][columnIndex-1]
            @clickOnWeightElement="openElement"
            @clickOnMineElement="openElement"
            @clickOnBlankElement="openBlankArea" />
      </tr>
    </tbody>
  </table>
</template>

<script>
import MatrixElement from "./MatrixElement.vue";

export default {
  name: "MatrixMines",
  components: {
    MatrixElement
  },
  data() {
    return {
      totalMines: 10,
      matrixMines: this.initalMatrixMines().matrixMines,
      positionMines: this.initalMatrixMines().positionMines,
      isOpenMatrixMines: this.initalMatrixMines().isOpenMatrixMines
    };
  },
  methods: {
    openBlankArea: function(rowIndex, columnIndex) {
      for (let i = -1; i <= 1; i++) {
        for (let j = -1; j <= 1; j++) {
          let xi = rowIndex + i;
          let yj = columnIndex + j;
          if (xi >= 0 && xi < this.matrixMines.length && yj >= 0 && yj < this.matrixMines.length) {
            if (this.isOpenMatrixMines[xi][yj] == 0) {
              this.openElement(xi, yj)
              if (this.matrixMines[xi][yj] === 0) {
                this.openBlankArea(xi, yj);
              }
            }
          }
        }
      }
    },
    openElement: function(rowIndex, columnIndex) {
      this.isOpenMatrixMines[rowIndex].splice(columnIndex, 1 , 1);
    },
    pushMines: function() {},
    checkElementIsMine: function(rowIndex, columnIndex) {
      let position = { x: rowIndex, y: columnIndex };
      for (let i = 0; i < this.positionMines.length; i++) {
        if (this.comparePosition(this.positionMines[i], position)) {
          return -1;
        }
      }
      return 0;
    },
    initalMatrixMines: function() {
      let positionMines = [
        { x: 1, y: 5 },
        { x: 2, y: 2 },
        { x: 2, y: 5 },
        { x: 3, y: 2 },
        { x: 3, y: 5 },
        { x: 4, y: 1 },
        { x: 6, y: 0 },
        { x: 6, y: 6 },
        { x: 7, y: 5 },
        { x: 7, y: 6 }
      ];
      let matrixMines = [];
      let isOpenMatrixMines = [];
      for (let i = 0; i <= 7; i++) {
        let matrixMinesRow = [];
        let isOpenMatrixMinesRow = [];
        for (let j = 0; j <= 7; j++) {
          matrixMinesRow.push(0);
          isOpenMatrixMinesRow.push(0);
        }
        matrixMines.push(matrixMinesRow);
        isOpenMatrixMines.push(isOpenMatrixMinesRow);
      }
      for (let i = 0; i < positionMines.length; i++) {
        let x = positionMines[i].x;
        let y = positionMines[i].y;
        matrixMines[x][y] = -1;
      }
      this.calculateElementWeight(positionMines, matrixMines);
      return {
        matrixMines,
        positionMines,
        isOpenMatrixMines
      };
    },
    updateRelateElement: function(x, y, matrixMines) {
      for (let i = -1; i <= 1; i++) {
        for (let j = -1; j <= 1; j++) {
          let xi = x + i;
          let yj = y + j;
          if (xi >= 0 && xi < matrixMines.length && yj >= 0 && yj < matrixMines.length) {
            if (matrixMines[xi][yj] >= 0) {
              matrixMines[x + i][y + j] ++;
            }
          }
        }
      }
    },
    calculateElementWeight: function(positionMines, matrixMines) {
      for (let i = 0; i < positionMines.length; i++) {
        let x = positionMines[i].x;
        let y = positionMines[i].y;
        this.updateRelateElement(x, y, matrixMines);
      }
    },
    comparePosition: function(fPosition, sPosition) {
      if (fPosition.x === sPosition.x && fPosition.y === sPosition.y) {
        return true;
      }
      return false;
    }
  }
};
</script>

<style scoped>
table {
  border-spacing: 3px;
}
</style>
