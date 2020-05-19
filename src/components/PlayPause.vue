<template>
    <div class="play-pause">
        <button
                id="play"
                :class="[
            nowPlaying
              ? 'https://res.cloudinary.com/bhankypsalm/image/upload/v1577821230/pause_a5xv9v.svg'
              : 'https://res.cloudinary.com/bhankypsalm/image/upload/v1578308363/play_ctmnhk.svg',
              (nowPlaying ? 'fe fe-pause' : 'fe fe-play')]"
                @click="playOrPauseAudio()"
                v-for="audio in audios"
                :key="audio.id"
                title="play/pause">
        </button>
    </div>
</template>

<script>
    export default {
        name: "PlayPause",
        props: {
            audios: Array,
            playOrPauseAudio: Function,
            nowPlaying: Boolean
        },
        data() {
            return {};
        },
    }
</script>

<style scoped>

</style>
