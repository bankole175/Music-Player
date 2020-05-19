<template>
  <div id="app">
    <div class="centerContainer">
      <a
        class="navIcon"
        v-on:click="isMenuActive = !isMenuActive"
        :class="{ isActive: isMenuActive }"
        title="Menu"
      >
        <span class="menu"></span>
        <span class="menu"></span>
        <span class="menu"></span>
      </a>
      <div class="songContainer" :class="{ isActive: isMenuActive }">
        <song-list :audios="audios" :isCurrentSong="isCurrentSong" :playNext="playNext"  />
      </div>

      <div class="main-body">

      </div>
      <div class="coverImage">
        <img @load="onLoadCoverImage()" :src="audios[currentSong].coverImage" />
      </div>
      <div class="audioDetails">
        <span class="artist">{{ audios[currentSong].artist }} - </span>
        <span class="song-title">{{ audios[currentSong].title }}</span>
      </div>
      <div id="buttons">
        <div class="showSongTime">
          <span class="currentTime">{{ currentTime | timing }}</span>
          <span class="totalTime">{{ trackTiming | timing }}</span>
        </div>
        <progress-bar :currentProgressBar="currentProgressBar" />
        <shuffle />
        <previous :previous="previous" />
        <play-pause :audios="audios" :playOrPauseAudio="playOrPauseAudio" :nowPlaying="nowPlaying" />
        <next :next="next" />
        <headset />
      </div>
      <waves />
    </div>
    <div class="topContainer"></div>
  </div>
</template>

<script>
import SongList from './components/SongList.vue'
import Waves from "./components/layout/Waves.vue";
import Next from "./components/Next.vue";
import Previous from "./components/Previous.vue";
import ProgressBar from "./components/ProgressBar.vue";
import Shuffle from "./components/Shuffle.vue";
import Headset from "./components/Headset.vue";
import PlayPause from "./components/PlayPause.vue";

export default {
  name: "app",
  components: {
    SongList,
    Waves,
    Next,
    Previous,
    ProgressBar,
    Shuffle,
    Headset,
    PlayPause,
  },
  data() {
    return {
      audio: "",
      coverImg: false,
      nowPlaying: false,
      notPlaying: false,
      currentSong: 0,
      audioFile: "",
      trackTiming: 0,
      currentTime: 0,
      currentProgressBar: 0,
      checkTrackIndex: "",
      isMenuActive: false,
      playlist: false,
      audios: [
        {
          id: "Tim",
          artist: "Godfrey",
          title: "Take me there",
          coverImage:
            "https://res.cloudinary.com/bhankypsalm/image/upload/c_scale,h_200,q_75,w_250/v1577875865/austin-neill-hgO1wFPXl3I-unsplash_qa5tk2.jpg",
          song:
            "https://res.cloudinary.com/bhankypsalm/video/upload/v1577817442/scifi001_xwfuio.mp3",
          isPlaying: false
        },
        {
          id: "Teni",
          artist: "Teniola",
          title: "Billionaire",
          coverImage:
            "https://res.cloudinary.com/bhankypsalm/image/upload/c_scale,h_200,q_75,w_250/v1577876305/hanny-naibaho-aWXVxy8BSzc-unsplash_p3iabx.jpg",
          song:
            "https://res.cloudinary.com/bhankypsalm/video/upload/v1577872910/scifi004_qoukl1.mp3",
          isPlaying: false
        },
        {
          id: "Travis",
          artist: "Travis Green",
          title: "You waited",
          coverImage:
            "https://res.cloudinary.com/bhankypsalm/image/upload/c_scale,h_200,q_75,w_250/v1577875984/hector-bermudez-iIWDt0fXa84-unsplash_qwzomw.jpg",
          song:
            "https://res.cloudinary.com/bhankypsalm/video/upload/v1577872872/musical006_ulrtdj.mp3",
          isPlaying: false
        },
        {
          id: "Oloyede",
          artist: "Elijah Oloyede",
          title: "Baba Oh",
          coverImage:
            "https://res.cloudinary.com/bhankypsalm/image/upload/c_scale,h_200,q_75,w_250/v1577875922/akshar-dave-sNut2MqSmds-unsplash_fxwvr5.jpg",
          song:
            "https://res.cloudinary.com/bhankypsalm/video/upload/v1577872888/musical008_mhj03j.mp3",
          isPlaying: false
        }
      ]
    };
  },

  mounted() {
    this.playNext();
    this.audio.loop = false;
  },
  filters: {
    timing(s) {
      return (s - (s %= 60)) / 60 + (9 < s ? ":" : ":0") + s;
    }
  },

  methods: {
    playOrPauseAudio() {
      if (
        this.notPlaying == true &&
        this.currentSong + 1 == this.audios.length
      ) {
        this.currentSong = 0;
        this.playNext();
      }
      if (!this.nowPlaying) {
        this.getSongTimingEverySeconds(true);
        this.nowPlaying = true;
        this.audio.play();
      } else {
        this.pauseAudio();
      }
      this.notPlaying = false;
    },
    pauseAudio() {
      this.audio.pause();
      this.nowPlaying = false;
      this.pausedAudio();
    },
    pausedAudio() {
      clearTimeout(this.checkTrackIndex);
    },
    previous() {
      if (this.currentSong > 0) this.playNext(this.currentSong - 1);
    },
    next() {
      if (this.currentSong < this.audios.length - 1) {
        this.playNext(this.currentSong + 1);
      }
    },
    playNext(index) {
      let wasPlayed = this.nowPlaying;
      this.coverImg = false;
      if (index !== undefined) {
        this.pauseAudio();
        this.currentSong = index;
      }
      this.audioFile = this.audios[this.currentSong].song;
      this.audio = new Audio(this.audioFile);
      let nowLocal = this;
      this.audio.addEventListener("loadedmetadata", function() {
        nowLocal.trackTiming = Math.round(this.duration);
      });
      this.audio.addEventListener("stopped", this.nowEnded);
      if (wasPlayed) {
        this.playOrPauseAudio();
      }
    },
    isCurrentSong(index) {
      if (this.currentSong == index) {
        return true;
      }
      return false;
    },
    onLoadCoverImage() {
      this.coverImg = true;
    },
    getSongTimingEverySeconds() {
      this.checkTrackIndex = setTimeout(
        function() {
          this.currentTime = this.audio.currentTime;
          this.currentProgressBar =
            (this.audio.currentTime / this.trackTiming) * 100;
          this.getSongTimingEverySeconds(true);
        }.bind(this),
        1000
      );
    },
  },
  watch: {
    currentTime() {
      this.currentTime = Math.round(this.currentTime);
    }
  }
};
</script>

<style>
#app {
  position: relative;
}
.topContainer {
  clip-path: polygon(0 0, 0 18%, 18% 30%, 54% 37%, 100% 44%, 100% 41%, 100% 0);
  background-image: linear-gradient(to top, #4481eb 0%, #04befe 100%);
  background-size: cover;
  width: 100%;
  height: 700px;
}

.centerContainer {
  width: 300px;
  height: 75%;
  min-width: 260px;
  box-shadow: 5px 2px 5px 5px rgba(175, 173, 173, 0.3);
  background-color: #fff;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 999;
  overflow: hidden;
}

.navIcon {
  width: 20px;
  height: 20px;
  position: absolute;
  top: 1.7%;
  left: 1.8%;
  transform: rotate(0deg);
  transition: 0.25s ease-in-out;
  cursor: pointer;
}

.menu {
  display: block;
  position: absolute;
  height: 2px;
  width: 100%;
  background: rgba(0, 0, 0, 0.75);
  border-radius: 6px;
  opacity: 1;
  left: 0;
  transform: rotate(0deg);
  transition: 0.5s ease-in-out;
  cursor: pointer;
}

.menu:nth-child(1) {
  top: 0px;
}

.menu:nth-child(2) {
  top: 5px;
}

.menu:nth-child(3) {
  top: 10px;
}

.isActive .menu:nth-child(1) {
  top: 5px;
  transform: rotate(135deg);
}

.isActive .menu:nth-child(2) {
  opacity: 0;
  left: -60px;
}

.isActive .menu:nth-child(3) {
  top: 5px;
  transform: rotate(-135deg);
}

.songContainer {
  color: rgba(0, 0, 0, 0.75);
  background: #fff;
  width: 100%;
  height: 80vh;
  transition: 0.5s;
  transform: translateX(-200%);
  position: absolute;
  margin-top: 2.7rem;
  z-index: 999;
  will-change: transform;
}

.songContainer.isActive {
  transform: translateX(0);
}

.songListHeading {
  font-weight: 500;
  font-size: 1.2rem;
  text-align: center;
}

.playlistButton button {
  position: absolute;
  right: 1.5%;
  border-radius: 0.4rem;
  padding: 0.25rem;
  background: #00b2fa;
  color: #fff;
  border: none;
}

.songList {
  margin-bottom: 1.5rem;
  margin-left: 3%;
  text-align: center;
  border-left: 0.1rem solid transparent;
  transition: 0.2s;
  color: rgba(0, 0, 0, 1);
  font-size: 1rem;
  cursor: pointer;
}

.coverImage {
  position: absolute;
  max-width: 100%;
  top: 8%;
  left: 10%;
  z-index: 99;
}
.audioDetails {
  position: absolute;
  top: 50%;
  left: 25%;
  animation: scrolling-text 15s linear infinite;
}
@keyframes scrolling-text {
  from {
    transform: translateX(100%);
  }
  to {
    transform: translateX(-100%);
  }
}

.showSongTime {
  position: absolute;
  top: -85%;
  left: 5%;
  display: flex;
  width: 89%;
  height: 1rem;
  justify-content: space-between;
  color: #fff;
}

.currentProgressBar {
  width: 90%;
  background-color: #fff;
  position: absolute;
  top: -35%;
  left: 5%;
  cursor: pointer;
}
.progressBar {
  width: 0px;
  background-color: rgb(252, 128, 225);
  height: 2.5px;
}

#buttons {
  height: 50px;
  width: 100%;
  margin-top: 50px;
  position: relative;
  top: 83%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 99;
}

#play,
#next,
#pre,
#shuffle,
#headset {
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}


.fe {
  font-size: 26px;
  color: #ffffff;
}

#next {
  left: 70%;
}

#pre {
  left: 30%;
}

#shuffle {
  left: 10%;
}

#headset {
  left: 85%;
}

.st0 {
  fill: url(#SVGID_1_);
}
.st1 {
  fill: url(#SVGID_2_);
}
.st2 {
  fill: url(#SVGID_3_);
}

.waves {
  position: relative;
  top: 208px;
  -webkit-transition: top 0.2s ease-in-out;
  transition: top 0.2s ease-in-out;
}

@media screen and (max-width: 400px) {
  .centerContainer {
    width:60%;
    height: 70%;
    top: 45%;
  }

  .coverImage {
    position: absolute;
    width: 10px;
    top: 8%;
    left: 2%;
    right: 7%;
  }
}
</style>
