<template>
  <div class="hello">
    <table>
      <tr>
        <th/>
        <th><button @mousedown="up" @mouseup="release_up">⬆️</button></th>
        <th/>
      </tr>
      <tr>
        <th><button @mousedown="left" @mouseup="release_left">⬅️</button></th>
        <th/>
        <th> <button @mousedown="right" @mouseup="release_right">➡️</button></th>
      </tr>
      <tr>
        <th/>
        <th><button @mousedown="down" @mouseup="release_down">⬇️</button></th>
        <th/>
      </tr>
    </table> 
  </div>
</template>

<script>
export default {
  name: 'ButtonJoystick',
  methods: {
    up() { this.$emit('up') },
    down() { this.$emit('down') },
    left() { this.$emit('left') },
    right() { this.$emit('right') },
    release_up() { this.$emit('rel_up') },
    release_down() { this.$emit('rel_down') },
    release_left() { this.$emit('rel_left') },
    release_right() { this.$emit('rel_right') },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
