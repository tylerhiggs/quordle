<template>
    <div class="grid-container">
        <div v-for="guess in guesses" :key="guess">
            <GuessedLine 
                v-bind:guess="guess"
                v-bind:answer="answer"
                />
        </div>        
        <div v-if="!completed" :style="isCurrentGuessWord() ? '' : 'color: red;'">
            <GuessingLine 
                v-bind:guess="currentGuess || ' '"
            />
        </div>
        <div v-for="_ in 9-guesses.length" :key="_">
            <div class="empty-box">
            </div>
        </div>
    </div>
</template>

<script>
import GuessedLine from "./GuessedLine.vue";
import GuessingLine from "./GuessingLine.vue";

export default {
    name: 'SingleQuadrant',
    components: {
        GuessedLine,
        GuessingLine,
    },
    props: {
        answer: String,
        guesses: Array,
        currentGuess: String,
        completed: Boolean,
        isCurrentGuessWord: Function,
    },
    data() {
        return {
        }
    },
}


</script>

<style>

.grid-container {
    display: grid;
    grid-template-columns: 1fr;
    grid-template-rows: repeat(9, 1fr);
    grid-gap: 2px;
    height: 95%;
}
.empty-box {
    grid-row: 1 / span 5;
    background: lightgrey;
    border-radius: 0.25rem;
    height: 100%;
    margin-left: 2px;
    margin-right: 2px;
}
</style>
