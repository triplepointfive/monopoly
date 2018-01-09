<template lang="slm">
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
