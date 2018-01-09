<template lang='slm'>
  td.corner :class='image'
    span.player v-for='player in playersIds' :class='"id" + player'
</template>

<script>

export default {
  name: 'Cell',
  props: ['i', 'j', 'players', 'image'],
  computed: {
    playersIds() {
      let ids = []

      this.players.forEach((player, index) => {
        if (this.i == player.i && this.j == player.j) {
          ids.push(index)
        }
      })

      return ids
    }
  }
}
</script>

<style lang="scss">
td {
  border: 1px solid black;
  width: 50px;
  height: 50px;

  &.corner {
    background-position: center center;
    background-size: 100% 100%;
  }

  &.parking {
    background-image: url(/parking.png);
  }

  &.police {
    background-image: url(/police.svg);
  }

  &.jail {
    background-image: url(/jail.png);
  }

  &.salary {
    background-image: url(/salary.png);
  }

  img {
    max-width: 100%;
    max-height: 100%;
  }
}

.player {
  width: 20px;
  height: 20px;
  border-radius: 10px;
  display: inline-block;

  &.id0 {
    background-color: red;
  }

  &.id1 {
    background-color: green;
  }

  &.id2 {
    background-color: blue;
  }

  &.id3 {
    background-color: purple;
  }
}
</style>
