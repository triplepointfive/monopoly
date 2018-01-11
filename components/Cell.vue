<template lang='slm'>
  td[
    v-if='buildingCell'
    ]
    .building-card
      .header :class='"-set" + setId'
      .body
        .title
          | {{ text }}
        .image
          img :src='image'
        .price
          | {{ cost }}K$
    Player v-for='playerId in playersIds' :id='playerId' :key='playerId'

  td.corner.image[
    v-else
    :style='backgroundStyle'
    ]
    Player v-for='playerId in playersIds' :id='playerId' :key='playerId'
</template>

<script>
import Player from './Player'

export default {
  name: 'Cell',
  props: ['i', 'j', 'players', 'image', 'text', 'setId', 'cost'],
  components: {
    Player
  },
  computed: {
    playersIds() {
      let ids = []

      this.players.forEach(player => {
        if (this.i == player.i && this.j == player.j) {
          ids.push(player.id)
        }
      })

      return ids
    },
    backgroundStyle() {
      return `background-image: url(${this.image})`
    },
    buildingCell() {
      return this.setId
    }
  }
}
</script>

<style lang="scss">
.image {
  background-position: center center;
  background-size: 100% 100%;
}

.-set1 {
  background-color: #DF3FEE;
}
.-set2 {
  background-color: #7F211B;
}
.-set3 {
  background-color: #C70C13;
}
.-set4 {
  background-color: #D7E60C;
}
.-set5 {
  background-color: #181798;
}
.-set6 {
  background-color: #F8A9FC;
}
.-set7 {
  background-color: #35EBD4;
}
.-set8 {
  background-color: #31FA1D;
}

.building-card {
  height: 100%;
  width: 100%;
  position: absolute;
  top: 0;
  z-index: -1;

  > .header {
    height: 10px;
  }

  > .body {
    position: relative;
    height: 90px;

    > .title {
      height: 20%;
      text-align: center;
    }

    > .image {
      height: 60%;

      img {
        width: 100%;
        height: 100%;
      }
    }

    > .price {
      height: 20%;
      text-align: center;
    }
  }
}
</style>
