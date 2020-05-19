<template>
    <button id="next" @click="next()" title="next">
        <svg
                xmlns="http://www.w3.org/2000/svg"
                width="35"
                height="35"
                fill="#ffffff"
                viewBox="0 0 48 48"
        >
            <path d="M12 36l17-12-17-12v24zm20-24v24h4V12h-4z" />
        </svg>
    </button>
</template>

<script>
    export default {
        name: "Next",
        props: {
            next: Function,
        }
    }
</script>

<style scoped>

</style>
