<template lang='slm'>
  span.player :class='"id" + id'
</template>

<script>
export default {
  name: 'Player',
  props: ['id']
}
</script>

<style lang='scss'>
.player {
  width: 20px;
  height: 20px;
  border: solid black 3px;
  border-radius: 10px;
  display: inline-block;

  &.id1 {
    background-color: red;
  }

  &.id2 {
    background-color: green;
  }

  &.id3 {
    background-color: blue;
  }

  &.id4 {
    background-color: purple;
  }
}
</style>
