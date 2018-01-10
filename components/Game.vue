<template lang="slm">
  div
    div style='float: left;'
      table.desk
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
    div
      table
        tr v-for='(player, i) in players' :key='i'
          td
            span[
              v-show='player == currentPlayer'
              ]
              | ➡
          td
            DeskPlayer :id='i'
          td
            span v-for='n in player.level * 2 - 1'
              | ★
          td
            | {{ player.money }}K$

</template>

<script>
import Dice from './Dice'
import DeskCell from './Cell'
import DeskPlayer from './Player'

const randomInteger = function (min, max) {
  return Math.floor(min + Math.random() * (max + 1 - min))
}

const randDice = function() { return randomInteger(1, 5) }

// TODO: Make an interface
class Cell {
  constructor(moveDir, image) {
    this.moveDir = moveDir
    this.image = image
  }

  onStepOver(player) {
    // Does nothing by default
  }

  onStop(player) {
    // Does nothing by default
  }
}

class BuildingCell extends Cell {
  constructor(moveDir, setId, image, text) {
    super(moveDir, image)
    this.setId = setId
    this.text = text
  }
}

class DiamondCell extends Cell {
  constructor(moveDir) {
    super(moveDir, '/diamond.jpg')
  }
}

class ChanceCell extends Cell {
  constructor(moveDir) {
    super(moveDir, '/chance.png')
  }
}

class SalaryCell extends Cell {
  constructor(moveDir) {
    super(moveDir, '/salary.png')
  }

  onStepOver(player) {
    switch(player.level) {
    case 1:
      player.money += 150
      break
    case 2:
      player.money += 200
      break
    case 3:
      player.money += 250
      break
    default:
      throw(`Unknown player level ${player.level}`)
    }

    // TODO: Ask if a player would like to upgrade
    if (Math.random() > 0.5 && player.level < 3) {
      player.level += 1
      player.money -= 50
    }
  }
}

class Player {
  constructor() {
    this.i = 8
    this.j = 8
    this.money = 372
    this.level = 1
  }
}

const parkingCell = new Cell('right', '/parking.png')
const jailCell = new Cell('up', '/jail.png')
const policeCell = new Cell('down', '/police.svg')
const salaryCell = new SalaryCell('/left.png')

let cells = new Array(9);
for (let i = 0; i < 9; i++) {
  cells[i] = new Array(9);
}

cells[8][8] = salaryCell
cells[8][7] = new BuildingCell('left', 1, '/buildings/DogHouse.png', 'Конура')
cells[8][6] = new DiamondCell('left')
cells[8][5] = new BuildingCell('left', 1, '/buildings/TreeHouse.png', 'Дом на дереве')
cells[8][4] = new BuildingCell('left', 2, '/buildings/Igloo.png', 'Иглу')
cells[8][3] = new ChanceCell('left')
cells[8][2] = new BuildingCell('left', 2, '/buildings/Hut.png', 'Палатка')
cells[8][1] = new BuildingCell('left', 2, '/buildings/Shed.png', 'Сарай')

cells[8][0] = jailCell
cells[7][0] = new BuildingCell('up', 3, '/buildings/Museum.png', 'Музей')
cells[6][0] = new BuildingCell('up', 3, '/buildings/LightHouse.png', 'Маяк')
cells[5][0] = new BuildingCell('up', 3, '/buildings/School.png', 'Школа')
cells[4][0] = new BuildingCell('up', 4, '/buildings/Shop.png', 'Магазин')
cells[3][0] = new DiamondCell('up')
cells[2][0] = new BuildingCell('up', 4, '/buildings/CityHall.png', 'Ратуша')
cells[1][0] = new BuildingCell('up', 4, '/buildings/PostOffice.png', 'Почта')

cells[0][0] = parkingCell
cells[0][1] = new BuildingCell('right', 5, '/buildings/Embassy.png', 'Посольство')
cells[0][2] = new ChanceCell('right')
cells[0][3] = new BuildingCell('right', 5, '/buildings/Temple.png', 'Храм')
cells[0][4] = new BuildingCell('right', 5, '/buildings/Church.png', 'Церковь')
cells[0][5] = new BuildingCell('right', 6, '/buildings/Hospital.png', 'Больница')
cells[0][6] = new BuildingCell('right', 6, '/buildings/OfficeBuilding.png', 'Офис')
cells[0][7] = new BuildingCell('right', 6, '/buildings/Tower.png', 'Башня')

cells[0][8] = policeCell
cells[1][8] = new BuildingCell('down', 7, '/buildings/Factory.png', 'Фабрика')
cells[2][8] = new BuildingCell('down', 7, '/buildings/Pyramid.png', 'Пирамида')
cells[3][8] = new DiamondCell('down')
cells[4][8] = new BuildingCell('down', 7, '/buildings/Castle.png', 'Замок')
cells[5][8] = new ChanceCell('down')
cells[6][8] = new BuildingCell('down', 8, '/buildings/Stadium.png', 'Стадион')
cells[7][8] = new BuildingCell('down', 8, '/buildings/Skyscraper.png', 'Небоскрёб')

export default {
  name: 'Game',
  data() {
    return {
      dices: [6, 6],
      cells: cells,
      turn: 0,
      players: [new Player(), new Player(), new Player(), new Player()]
    }
  },
  components: {
    DeskCell,
    Dice,
    DeskPlayer
  },
  methods: {
    roll() {
      this.dices = [randDice(), randDice()]
      this.movePlayer();
    },
    movePlayer() {
      let steps = this.dices.reduce((acc, val) => acc + val)

      let cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]

      const stepIterval = setInterval(() => {
        switch (cell.moveDir) {
          case 'up':
            this.currentPlayer.i -= 1
            break
          case 'down':
            this.currentPlayer.i += 1
            break
          case 'right':
            this.currentPlayer.j += 1
            break
          case 'left':
            this.currentPlayer.j -= 1
            break
          default:
            throw(`Unknown moveDir ${cell.moveDir}`)
        }

        cell = this.cells[this.currentPlayer.i][this.currentPlayer.j]

        console.log(this.currentPlayer.i, this.currentPlayer.j)

        steps -= 1

        if (steps <= 0) {
          clearInterval(stepIterval)

          cell.onStop(this.player)

          const [d1, d2] = this.dices

          if (d1 !== d2) {
            // TODO: If dices are same for the third time go to jail
            this.turn += 1
          }
        } else {
          cell.onStepOver(this.currentPlayer)
        }
      }, 100)
    }
  },
  computed: {
    currentPlayer() {
      return this.players[this.turn % this.players.length]
    }
  }
}
</script>

<style lang="scss">
</style>
