<template>
    <td @click="handleClickEvent" :class="{'blank-element': this.weight == 0}">
      <template v-if="this.isOpened == 1">
        <template v-if="this.checkMine()"><i class="fas fa-atom"></i></template>
        <template v-if="this.weight > 0">{{this.weight}}</template>
      </template>
    </td>
</template>

<script>
export default {
    name: 'MatrixElement',
    props: [
      'weight',
      'rowIndex',
      'columnIndex',
      'isOpened'
    ],
    methods: {
      handleClickEvent() {
        if (this.weight === 0) {
          this.$emit('clickOnBlankElement', this.rowIndex, this.columnIndex);
        } else if (this.weight === -1) {
          this.$emit('clickOnMineElement', this.rowIndex, this.columnIndex);
        } else {
          this.$emit('clickOnWeightElement', this.rowIndex, this.columnIndex);
        }

      },
      checkMine() {
        if (this.weight === -1) {
            return true
        }
        return false;
      }
    }
};
</script>

<style scoped>
td {
  width: 40px;
  height: 40px;
  border-radius: 1px;
  text-align: center;
  font-weight: bold;
  font-size: 24px;
  background: #00000040;
}
.blank-element {
  background: #00000020;
}
</style>
