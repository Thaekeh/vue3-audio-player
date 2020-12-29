<template>
  <head>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />
  </head>
  <div id="audio-player">
    <div class="image-container">
      <transition-group :name="transitionName">
        <div
          class="image"
          :style="{ backgroundImage: `url(${currentSong.image})` }"
        ></div>
      </transition-group>
    </div>
    <h1 id="title">{{ currentSong.title }}</h1>
    <h2 id="artist">{{ currentSong.artist }}</h2>
    <button class="button" @click="lowerVolume">
      <i class="fa fa-volume-down"></i>
    </button>
    <button class="button" @click="decrement">
      <i class="fa fa-step-backward"></i>
    </button>
    <button class="button" @click="togglePlay">
      <i id="icon" class="fa" :class="playing ? 'fa-pause' : 'fa-play'"></i>
    </button>
    <button class="button" @click="increment">
      <i class="fa fa-step-forward"></i>
    </button>
    <button
      class="button"
      @click="raiseVolume"
    >
      <i class="fa fa-volume-up"></i>
    </button>
    <div id="timeline" @click="logEvent" ref="timeline">
      <div
        @mousedown="mouseDown"
        id="playhead"
        ref="playhead"
        :style="{ marginLeft: playheadPosition + 'px' }"
      ></div>
    </div>
    <p>
      {{ currentTime }} /
      {{
        audio.duration
          ? `${Math.floor(audio.duration / 60)}:${Math.floor(
              audio.duration % 60
            )} `
          : 0
      }}
    </p>
    <p>{{ itemWidth }}</p>
  </div>
</template>

<script lang="ts">
export default {
  name: "AudioPlayer",
  data() {
    return {
      volumeSlider: 0.5,
      volumeHover: false,
      transitionName: null,
      playing: false,
      currentTime: 0,
      currentSongIndex: 0,
      audio: "",
      playlist: [
        {
          title: "Bohemian Rhapsody",
          artist: "Queen",
          image: "https://archive.org/services/img/QueenQueenI",
          song: "https://www.soundjay.com/free-music/sounds/iron-man-01.mp3"
        },
        {
          title: "Tron",
          artist: "Daft Punk",
          image: "https://archive.org/services/img/Scan1160",
          song: "https://www.soundjay.com/free-music/sounds/destination-01.mp3"
        }
      ]
    };
  },
  created() {
    console.clear();
    let self = this;
    this.audio = new Audio();
    this.audio.src = this.currentSong.song;
    this.audio.ontimeupdate = function() {
      self.updateTime();
    };
  },
  methods: {
    setVolume() {
      // console.log('Before: ' + this.audio.volume);
      this.audio.volume = this.volumeSlider / 100;
      console.log("After: " + this.audio.volume);
    },
    lowerVolume() {
      if (this.audio.volume >= 0.1) {
        this.audio.volume = this.audio.volume - 0.1;
      }
    },
    raiseVolume() {
      if (this.audio.volume <= 0.9) {
        this.audio.volume = this.audio.volume + 0.1;
      }
    },
    logEvent(e) {
      // console.log("Ref width: " + this.$refs.timeline.clientWidth);
      // console.log("Current Time: " + this.audio.currentTime);

      // // let width = e.target.clientWidth;

      let clickPosition = e.clientX;
      console.log("Click Position: " + clickPosition);

      // let elementPosition = this.getPosition(e.target);
      // console.log("Element Position: " + elementPosition);

      // let audioPosition = (clickPosition - elementPosition) / width;

      // console.log(this.audio.duration);

      // let newTime = audioPosition * this.audio.duration;
      // console.log("New Time: " + newTime);

      // this.audio.currentTime = newTime;

      // let playhead = document.getElementById("playhead");
      // let position = elementPosition + width - clickPosition;
      // playhead.style.marginLeft = position + "px";

      // console.log(e.clientX);
      // let clickPosition = e.clientX;
      // console.log("Width: " + e.target.clientWidth);
      // console.log("Position: " + position);
      // console.log(position * this.audio.duration);
      // this.audio.currentTime = (position * this.audio.duration);
    },
    mouseDown() {
      window.addEventListener("mousemove", this.moveplayhead, true);
    },
    moveplayhead() {
      return "hi";
    },
    getPosition(el) {
      return el.getBoundingClientRect().left;
    },
    togglePlay() {
      if (this.playing) {
        this.audio.pause();
      } else {
        this.audio.play();
      }
      this.playing = !this.playing;
    },
    increment() {
      this.currentSongIndex++;

      if (this.currentSongIndex + 1 > this.playlist.length) {
        this.currentSongIndex = 0;
      }

      this.audio.src = this.currentSong.song;
      if (this.playing) {
        this.audio.play();
      }
    },
    decrement() {
      this.transitionName = "scale-in";
      this.currentSongIndex--;

      if (this.currentSongIndex < 0) {
        this.currentSongIndex = this.playlist.length - 1;
      }

      this.audio.src = this.currentSong.song;
      if (this.playing) {
        this.audio.play();
      }
    },
    updateTime() {
      this.currentTime = Math.floor(this.audio.currentTime);
    }
  },
  computed: {
    currentSong() {
      return this.playlist[this.currentSongIndex];
    },
    // progress() {
    //   return (this.currentTime / this.audio.duration) * (this.$refs.timeline.clientWidth);
    // },
    audioDuration() {
      return this.audio.duration;
      // let minutes = Math.floor(this.audio.duration / 60);
      // let seconds = Math.floor(this.audio.duration % 60);
      // return `${Math.floor(this.audio.duration / 60)}:${Math.floor(this.audio.duration % 60)}`
      // return `${this.audio.duration / 60}:${this.audio.duration % 60}`
      // return `${minutes}:${seconds}`
      // return (Math.floor(this.audio.duration / 60))
    },
    playheadPosition: function() {
      // let width = this.$refs.timeline.clientWidth;
      // console.log(width);

      return this.currentTime / this.audio.duration;
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
  box-shadow: 0px 15px 35px -5px rgba(50, 88, 130, 0.32);
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
  font-size: 24px;
  margin: 0 20px;
  margin-top: 20px;
  padding: 10px 20px;
  transition: all 300ms;
  text-align: center;
  line-height: 50%;
  height: 60px;
  width: 60px;
  &:hover {
    border-radius: 100px;
    cursor: pointer;
    // border: 2px solid purple;
    background-color: rgba($color: #000, $alpha: 0.2);
  }
}

#timeline {
  width: 80%;
  height: 10px;
  background-color: rgba(0, 0, 0, 0.1);
  margin-left: auto;
  margin-right: auto;
  margin-top: 20px;
  border-radius: 10px;
}

#playhead {
  width: 15px;
  height: 15px;
  background-color: #000;
  border-radius: 10px;
  position: absolute;
  margin-top: -3px;
  transition: all 200ms;
  &:hover {
    cursor: pointer;
    transform: scale(1.1);
    background-color: #4adb4a;
  }
}

.scale-in-enter-active {
  transition: all 0.35s ease-in-out;
}
.scale-in-leave-active {
  transition: all 0.35s ease-in-out;
}
.scale-in-enter {
  transform: scale(1.2);
  pointer-events: none;
  opacity: 0;
}
.scale-in-leave-to {
  transform: scale(0.55);
  pointer-events: none;
  opacity: 0;
}
</style>
