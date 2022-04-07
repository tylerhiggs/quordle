<template>
    <div class="row-container">
        <div class="top-row-keys">
            <div class="single-key letter-key" v-for="i in 10" :key="i">
                <div :class="isDone(guesses, answers, i-1) ? 'alpha-z' : ''" @click="letterPressed(qwerty[i-1])">
                    <div class="color-squares">
                        <div v-for="j in 4" :key="j" :class="calcColor(guesses, answers, i-1, j-1)">
                        </div>
                    </div>
                    {{qwerty[i-1]}}
                </div>
            </div>
        </div>
        <div class="mid-row-keys">
            <div> 
            </div>
            <div class="single-key letter-key" v-for="i in 9" :key="i">
                <div :class="isDone(guesses, answers, i+9) ? 'alpha-z' : '' " @click="letterPressed(qwerty[i+9])">
                    {{qwerty[i+9]}}
                    <div class="color-squares">
                        <div v-for="j in 4" :key="j" :class="calcColor(guesses, answers, i+9, j-1)">
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="bottom-row-keys">
            <div class="single-key" @click="enterPressed">
                Enter
                <div class="color-squares">
                </div>
            </div>
            <div class="single-key letter-key" v-for="i in 7" :key="i">
                <div :class="isDone(guesses, answers, i+18) ? 'alpha-z' : ''" @click="letterPressed(qwerty[i+18])">
                    {{qwerty[i+18]}}
                    <div class="color-squares">
                        <div v-for="j in 4" :key="j" :class="calcColor(guesses, answers, i+18, j-1)">
                        </div>
                    </div>
                </div>
            </div>
            <div class="single-key" @click="backPressed">
                {{"<-"}}
                <div class="color-squares">
                </div>
            </div>
        </div>  
    </div>

</template>

<script>
export default {
    name: 'QuadrantsKeyboard',
    components: {
    },
    props: {
        answers: Array,
        guesses: Array,
    },
    created() {
        this.qwerty = [
            "Q",
            "W",
            "E",
            "R",
            "T",
            "Y",
            "U",
            "I",
            "O",
            "P",
            "A",
            "S",
            "D",
            "F",
            "G",
            "H",
            "J",
            "K",
            "L",
            "Z",
            "X",
            "C",
            "V",
            "B",
            "N",
            "M",
        ]
    },
    data() {
        return {
            currentGuess: "",
        }
    },
    methods: {
        isDone(gs, as, i) {
            //gs: guesses, as: answers, i: index
            const guessedLetter = gs.filter(g => g.includes(this.qwerty[i].toLowerCase())).length !== 0
            const answersLeft = as.filter(a => !gs.includes(a))
            const notInAnswer = answersLeft.filter(a => a.includes(this.qwerty[i].toLowerCase())).length === 0
            return guessedLetter && notInAnswer
        },
        quadrantClass(j) {
        return [
            "top-left-radius",
            "top-right-radius",
            "bottom-left-radius",
            "bottom-right-radius"
        ][j]
        },
        calcColor(gs, as, i, j) {
            const isDone = this.isDone(gs, as, i)
            const quadrantClass = this.quadrantClass(j)
            const letter = this.qwerty[i].toLowerCase()
            const answer = as[j]
            let isGreen = gs.filter(g => {
                for (let l = 0; l < g.length; l++) {
                    if (g[l] === letter && g[l] === answer[l]) {
                        return true
                    }
                }
                return false
            }).length !== 0
            if (isGreen && !isDone) {
                return "green-background " + quadrantClass
            }
            const isYellow = gs.filter(g => {
                for (let l = 0; l < g.length; l++) {
                    if ( g[l] === letter && answer.includes(g[l])) {
                        return true
                    }
                }
                return false
            }).length !== 0
            if (isYellow && !isDone) {
                return "yellow-background " + quadrantClass
            }
            if (!this.isDone(gs, as, i)) {
                return "white-background " + quadrantClass
            }
            else {
                return quadrantClass
            }
        },
        letterPressed(letter) {
            console.log("key pressed(keyboard vue): " + letter)
            this.emitter.emit('pressed', letter.toLowerCase())
        },
        backPressed() {
            this.emitter.emit("pressed" , "Backspace")
        },
        enterPressed() {
            this.emitter.emit("pressed", "Enter")
        },
    },
}


</script>

<style>
.rows-container {
    display: grid;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: 1fr;
    grid-row-start: 1;
}

.top-row-keys {
    display: grid;
    grid-template-columns: repeat(10, 1fr);
    grid-template-rows: 1fr;
    grid-column-start: 1;
    border-radius: 0.25rem;
}
.mid-row-keys {
    display: grid;
    grid-template-columns: 1fr repeat(9, 2fr) 1fr;
}
.bottom-row-keys {
    display: grid;
    grid-template-columns: 3fr repeat(7, 2fr) 3fr;
}

.single-key {
    position: relative;
    padding: 2px;
    margin: 2px;
    border-radius: 0.25rem;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
    cursor: pointer;
}

.letter-key {
    aspect-ratio: 1;

}

.alpha-z {
    opacity: 0.2;
    border-radius: 0.25rem;
}
.color-squares {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: whitesmoke;
    border-radius: 0.25rem;
    z-index: -1;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
}
.green-background {
    background-color: yellowgreen;
}
.yellow-background {
    background-color: yellow;
}
.white-background {
    background-color: whitesmoke;
}
.top-right-radius {
    border-radius: 0;
    border-top-right-radius: 0.25rem;
}
.top-left-radius {
    border-radius: 0;
    border-top-left-radius: 0.25rem;
}
.bottom-right-radius {
    border-radius: 0;
    border-bottom-right-radius: 0.25rem;
}
.bottom-left-radius {
    border-radius: 0;
    border-bottom-left-radius: 0.25rem;
}

</style>