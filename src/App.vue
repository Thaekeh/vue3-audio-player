<template>
  <AudioPlayer />
</template>

<script>
import AudioPlayer from './components/AudioPlayer.vue'

export default {
  name: 'App',
  components: {
    AudioPlayer
  }
}
</script>

<style>
body {
  background-color: #DFE7EF;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin-top: 60px;
}
</style>
