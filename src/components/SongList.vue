<template>
  <div class="song-list">
      <p class="songListHeading">Song List</p>
      <div class="audioPlayerList">
        <div
                class="songList"
                v-bind:class="{ 'isActive':isCurrentSong(index) }"
                @click="playNext(index)"
                v-for="(audio, index) in audios"
                :key="audio.id"
        >
            <span class="artist" @click="isMenuActive = !isMenuActive"
            >{{ audio.artist }} -
            </span>
          <span class="title" @click="isMenuActive = !isMenuActive">{{
              audio.title
            }}</span>
        </div>
      </div>
    </div>
</template>

<script>
export default {
  name: "SongList",
  props: {
    audios: Array,
    isCurrentSong: Function,
    playNext: Function,
  },
  data() {
    return {
      isMenuActive: false,
    };
  },
};
</script>

<style scoped></style>
