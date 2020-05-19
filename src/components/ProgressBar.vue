<template>
    <div class="currentProgressBar">
        <div
                class="progressBar"
                :style="{ width: currentProgressBar + '%' }"
        ></div>
    </div>
</template>

<script>
    export default {
        name: "ProgressBar",
        props: ['currentProgressBar'],
        data() {
            return {};
        },
    }
</script>

<style scoped>

</style>
