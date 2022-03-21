<template>
    <div class="word-grid">
        <div v-for="i in 5" :key="i">
            <div :class="this.letterColors[i-1] + ' flex-center'">
                <div>
                    {{guess[i-1].toUpperCase()}}
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
        answer: String,
    },
    mounted() {
        this.letterColor = {
            G: "green-background letter",
            Y: "yellow-background letter",
            B: "grey-background letter",
        }
        const greenFeq = {}
        const yellowFeq = {}
        // give colors completely based off whether letter matches or is in the word
        for (let i = 0; i < this.guess.length; i++) {
            let letter = this.guess[i]
            if (letter === this.answer[i]) {
                greenFeq[letter] = greenFeq[letter] ? greenFeq[letter] + 1 : 1
                this.letterColors.push(this.letterColor.G);
            } else if (this.answer.includes(letter)) {
                yellowFeq[letter] = yellowFeq[letter] ? yellowFeq[letter] + 1 : 1
                this.letterColors.push(this.letterColor.Y);
            } else {
                this.letterColors.push(this.letterColor.B);
            }
        }
        // now take away yellow tiles for letters that already have all green tiles filled in
        this.letterColors = this.letterColors.map((color, i) => {
            let letter = this.guess[i]
            if (color === this.letterColor.Y) {
                if (greenFeq[letter] === this.count(this.answer, letter)) {
                    return this.letterColor.B
                } else {
                    return this.letterColor.Y
                }
            } else {
                return color
            }
        })
    },
    data() {
        return {
            letterColors: []
        }
    },
    methods: {
        count(string, letter) {
            return string.split(letter).length - 1
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

.letter {
    grid-column: 1;
    margin: 2px;
    height: 95%;
}

.green-background {
    background-color: lightgreen;
    border-radius: 0.25rem;
}

.yellow-background {
    background-color: yellow;
    border-radius: 0.25rem;
}

.grey-background {
    background-color: lightgrey;
    border-radius: 0.25rem;
}

.flex-center {
    display: flex;
    justify-content: center;
    align-items: center;
}

</style>
