<template>
    <div>
    </div>
</template>

<script>

export default {
    name: 'KeyboardKeys',
    components: {
    },
    props: {
        letter: String,
        answer: String,
        guesses: Array,
    },
    data() {
        return {
            currentGuess: "",
        }
    },
}


</script>

<style>

.uses-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: repeat(2, 1fr);
    margin: 0;
    padding: 0;
}


</style>