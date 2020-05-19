<template>
    <button id="pre" @click="previous()" title="previous">
        <svg
                xmlns="http://www.w3.org/2000/svg"
                width="35"
                height="35"
                fill="#ffffff"
                viewBox="0 0 48 48"
        >
            <path d="M12 12h4v24h-4zm7 12l17 12V12z" />
        </svg>
    </button>
</template>

<script>
    export default {
        name: "Previous",
        props: {
            previous: Function,
        }
    }
</script>

<style scoped>

</style>
