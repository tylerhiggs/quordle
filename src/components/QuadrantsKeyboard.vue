<template>
    <div class="row-container">
        <div class="top-row-keys">
            <div class="single-key letter-key" v-for="i in 10" :key="i">
                {{qwerty[i-1]}}
            </div>
        </div>
        <div class="mid-row-keys">
            <div> 
            </div>
            <div v-for="i in 9" :key="i">
                <div class="single-key letter-key">
                    {{qwerty[i+9]}}
                </div>
            </div>
        </div>
        <div class="bottom-row-keys">
            <div class="single-key">
                Enter
            </div>
            <div v-for="i in 7" :key="i">
                <div class="single-key letter-key">
                    {{qwerty[i+18]}}
                </div>
            </div>
            <div class="single-key">
                {{"<-"}}
            </div>
        </div>  
    </div>

</template>

<script>
//import KeyboardKeys from "./KeyboardKeys.vue";  

    // <div v-for="r in 3" :key="r" class="rows-container">
    //     <div v-for="c in 10" :key="c" class="top-row-keys">
    //         <div v-if="r === 1" class="single-key">
    //             {{qwerty[c-1]}}
    //         </div>
    //     </div>
    // </div>

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
    background-color: whitesmoke;
    padding: 2px;
    margin: 2px;
    border-radius: 0.25rem;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
}

.letter-key {
    aspect-ratio: 1;
}

</style>