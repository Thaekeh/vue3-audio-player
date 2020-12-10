<template>
<head>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
</head>
  <div id="audio-player">

    <div class="image-container">
      <div class="image" :style="{backgroundImage: `url(${currentSong.image})`}"></div>

    </div>

    <!-- <img src="//unsplash.it/500/500"> -->
    <h1 id="title">{{ currentSong.title }}</h1>
    <h2 id="artist">{{ currentSong.artist }}</h2>
    <button class="button" @click="decrement">
      <i class="fa fa-step-backward"></i>
    </button>
    <button class="button" @click="togglePlay">
      <i id="icon" class="fa" :class="playing ? 'fa-pause' : 'fa-play'"></i>
    </button>
    <button class="button" @click="increment">
      <i class="fa fa-step-forward"></i>
    </button>
  </div>
</template>

<script lang="ts">
export default {
  name: "AudioPlayer",
  data() {
    return {
      playing: false,
      currentSongIndex: 0,
      playlist: [
        {
          title: 'Bohemian Rhapsody',
          artist: "Queen",
          image: "https://archive.org/services/img/QueenQueenI",
          song: ""
        },
        {
          title: 'Tron',
          artist: "Daft Punk",
          image:
            "https://archive.org/services/img/Scan1160",
          song: ""
        }
      ]
    };
  },
  mounted() {
    console.clear();
  },
  methods: {
    togglePlay() {
      this.playing = !this.playing
    },
    increment() {
      if (this.currentSongIndex < this.playlist.length - 1) {
        this.currentSongIndex++;
      }
    },
    decrement() {
      if (this.currentSongIndex > 0) {
        this.currentSongIndex--;
      }
    }
  },
  computed: {
    currentSong() {
      return this.playlist[this.currentSongIndex];
    }
  }
};
</script>

<style scoped lang="scss">
#audio-player {
  width: 500px;
  height: 600px;
  border-radius: 20px;
  background-color: #fff;
  margin-left: auto;
  margin-right: auto;
  margin-top: 200px;
}

.image-container {
  width: 300px;
  height: 300px;
  margin-left: auto;
  margin-right: auto;
  z-index: 5;
  position: relative;
  top: -70px;

  .image {
    background-repeat: no-repeat;
    background-position: center;
    width: 300px;
    height: 300px;
    background-size: cover;
    position: absolute;
    margin-left: auto;
    margin-right: auto;
    z-index: 5;
    border-radius: 15px;

    &:before {
        content: "";
        background: inherit;
        width: 100%;
        height: 100%;
        box-shadow: 0px 10px 40px 0px rgba(76, 70, 124, 0.5);
        display: block;
        z-index: 1;
        position: absolute;
        top: 30px;
        transform: scale(0.9);
        filter: blur(10px);
        opacity: 0.9;
        border-radius: 15px;
      }

    &:after {
        content: "";
        background: inherit;
        width: 100%;
        height: 100%;
        box-shadow: 0px 10px 40px 0px rgba(76, 70, 124, 0.5);
        display: block;
        z-index: 2;
        position: absolute;
        border-radius: 15px;
      }
  }
}

.button {
  background-color: rgba(0, 0, 0, 0);
  border: none;
  font-size: 32px;
  margin: 0 20px;
  margin-top: 50px;
  padding: 10px 20px;
  transition: all 300ms;
  text-align: center;
  line-height: 50%;
  height: 70px;
  width: 70px;
  &:hover {
    border-radius: 100px;
    cursor: pointer;
    // border: 2px solid purple;
    background-color: rgba($color: #000, $alpha: 0.2);
  }
}
</style>
