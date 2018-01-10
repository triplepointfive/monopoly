<template lang='slm'>
  td :style='backgroundStyle'
    Player v-for='playerId in playersIds' :id='playerId' :key='playerId'
</template>

<script>
import Player from './Player'

export default {
  name: 'Cell',
  props: ['i', 'j', 'players', 'image', 'text'],
  components: {
    Player
  },
  computed: {
    playersIds() {
      let ids = []

      this.players.forEach((player, index) => {
        if (this.i == player.i && this.j == player.j) {
          ids.push(index)
        }
      })

      return ids
    },
    backgroundStyle() {
      return `background-image: url(${this.image})`
    }
  }
}
</script>

<style lang="scss">
.desk {
  td {
    border: 1px solid black;
    width: 50px;
    height: 50px;

    background-position: center center;
    background-size: 100% 100%;
  }
}
</style>
