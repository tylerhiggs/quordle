<template>
    <div class="word-grid">
        <div v-for="i in guess.length" :key="i">
            <div class="flex-center guessing-letter">
                <div>
                    {{guess[i-1].toUpperCase()}}
                </div>
            </div>
        </div>
        <div v-for="i in 5-guess.length" :key="i">
            <div class="flex-center guessing-letter">
                <div>
                    {{' '}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>



export default {
    name: 'GuessedLine',
    components: {
    },
    props: {
        guess: String,
    },
    data() {
        return {
        }
    },
}


</script>

<style>

.word-grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-gap: 2px;
    height: 100%;
}

.guessing-letter {
    grid-column: 1;
    margin: 2px;
    height: 95%;
    border-radius: 0.25rem;
    background-color: whitesmoke;
}

.flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
}

</style>
