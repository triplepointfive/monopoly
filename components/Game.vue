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
            :text='cell.text'
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

class BuildingCell extends Cell {
  constructor(moveDir) {
    super(moveDir, null)
    this.text = 'home'
  }
}

class DiamondCell extends Cell {
  constructor(moveDir) {
    super(moveDir, 'diamond')
  }
}

class ChanceCell extends Cell {
  constructor(moveDir) {
    super(moveDir, 'chance')
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
cells[0][1] = new BuildingCell('right')
cells[0][2] = new ChanceCell('right')
cells[0][3] = new BuildingCell('right')
cells[0][4] = new BuildingCell('right')
cells[0][5] = new BuildingCell('right')
cells[0][6] = new BuildingCell('right')
cells[0][7] = new BuildingCell('right')

cells[0][8] = policeCell
cells[1][8] = new BuildingCell('down')
cells[2][8] = new BuildingCell('down')
cells[3][8] = new DiamondCell('down')
cells[4][8] = new BuildingCell('down')
cells[5][8] = new ChanceCell('down')
cells[6][8] = new BuildingCell('down')
cells[7][8] = new BuildingCell('down')

cells[8][8] = salaryCell
cells[8][7] = new BuildingCell('left')
cells[8][6] = new DiamondCell('left')
cells[8][5] = new BuildingCell('left')
cells[8][4] = new BuildingCell('left')
cells[8][3] = new ChanceCell('left')
cells[8][2] = new BuildingCell('left')
cells[8][1] = new BuildingCell('left')

cells[8][0] = jailCell
cells[7][0] = new BuildingCell('up')
cells[6][0] = new BuildingCell('up')
cells[5][0] = new BuildingCell('up')
cells[4][0] = new BuildingCell('up')
cells[3][0] = new DiamondCell('up')
cells[2][0] = new BuildingCell('up')
cells[1][0] = new BuildingCell('up')

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
      this.movePlayer();
    },
    movePlayer() {
      let steps = this.dices.reduce((acc, val) => acc + val)

      const stepIterval = setInterval(() => {
        const cell = this.cells[this.player.i][this.player.j]

        switch (cell.moveDir) {
          case 'up':
            this.player.i -= 1
            break
          case 'down':
            this.player.i += 1
            break
          case 'right':
            this.player.j += 1
            break
          case 'left':
            this.player.j -= 1
            break
          default:
            throw(`Unknown moveDir ${cell.moveDir}`)
        }

        steps -= 1

        if (steps <= 0) {
          clearInterval(stepIterval)
        }
      }, 100)
    }
  },
  computed: {
    player() {
      return this.players[0]
    }
  }
}
</script>

<style lang="scss">
</style>
