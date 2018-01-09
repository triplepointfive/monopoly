<template lang="slm">
  div
    table
      tr
        td
          img src='parking.png'
        td
        td
        td
        td
        td
        td
        td
        td
          img src='police.svg'
      tr
        td
        td colspan=7 rowspan=7
        td
      tr
        td
        td
      tr
        td
        td
      tr
        td
        td
      tr
        td
        td
      tr
        td
        td
      tr
        td
        td
      tr
        td
          img src='jail.png'
        td
        td
        td
        td
        td
        td
        td
        td
          img src='salary.png'
    h1
      Dice> v-for='(dice, i) in dices' :value='dice' :key="i"
      button @click="roll"
        | Role
</template>

<script>
import Dice from './Dice'

const randomInteger = function (min, max) {
  return Math.floor(min + Math.random() * (max + 1 - min))
}

const randDice = function() { return randomInteger(1, 6) }

export default {
  name: 'Game',
  data() {
    return {
      dices: [randDice(), randDice()]
    }
  },
  components: {
    Dice
  },
  methods: {
    roll() {
      this.dices = [randDice(), randDice()]
    }
  }
}
</script>

<style>
td {
  border: 1px solid black;
  width: 50px;
  height: 50px;
}

td img {
  max-width: 100%;
  max-height: 100%;
}
</style>
