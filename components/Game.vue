<template lang="slm">
  div
    table
      tr v-for='(row, i) in cells'
        template v-for='(cell, j) in row'
          DeskCell[
            v-if='cell'
            :i='i'
            :j='j'
            :players='players'
            :image='cell.image'
          ]
          td[
            rowspan=7
            colspan=7
            v-else-if='i == 1 && j == 1'
          ]
    h1
      Dice> v-for='(dice, i) in dices' :value='dice' :key="i"
      button @click="roll"
        | Role
</template>

<script>
import Dice from './Dice'
import DeskCell from './Cell'

const randomInteger = function (min, max) {
  return Math.floor(min + Math.random() * (max + 1 - min))
}

const randDice = function() { return randomInteger(1, 6) }

class Cell {
  constructor(moveDir, image) {
    this.moveDir = moveDir
    this.image = image
  }
}

class Player {
  constructor() {
    this.i = 8
    this.j = 8
  }
}

const parkingCell = new Cell('right', 'parking')
const jailCell = new Cell('up', 'jail')
const policeCell = new Cell('down', 'police')
const salaryCell = new Cell('left', 'salary')

let cells = new Array(9);
for (let i = 0; i < 9; i++) {
  cells[i] = new Array(9);
}

cells[0][0] = parkingCell
cells[0][8] = policeCell
cells[8][0] = jailCell
cells[8][8] = salaryCell

export default {
  name: 'Game',
  data() {
    return {
      dices: [randDice(), randDice()],
      cells: cells,
      players: [new Player(), new Player(), new Player(), new Player()]
    }
  },
  components: {
    DeskCell,
    Dice
  },
  methods: {
    roll() {
      this.dices = [randDice(), randDice()]
    }
  }
}
</script>

<style lang="scss">
</style>
