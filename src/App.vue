<template>
  <header class="header-container">
    <h1>Quordle</h1>
    <h2 :class="topMessageClass">{{this.topMessage}}</h2>
    <div class="new-game" @click="newGame">New Game </div>
  </header>

  <main class="main" v-on:keyup="keyPressed">
    
    <div class="four-wordles" ref="four-wordles">
      <div v-for="i in 4" :key="i">
        <SingleQuadrant 
          v-bind:answer="answers[i-1]"
          v-bind:guesses="completed[i-1] ? guesses.slice(0, finalIndices[i-1]) : guesses"
          v-bind:currentGuess="currentGuess"
          v-bind:completed="completed[i-1]"
          v-bind:isCurrentGuessWord="isCurrentGuessWord"
          class="single-wordle"
        />
      </div>
    </div>
    <div class="keyboard-container">
      <QuadrantsKeyboard
        v-bind:guesses="guesses"
        v-bind:answers="answers"
        class="keyboard"
      />
    </div>
  </main>
  <div v-if="this.gameOver" class="game-over-menu-container">
    <div class="game-over-menu-box">
    </div>
  </div>
</template>

<script>
import SingleQuadrant from './components/SingleQuadrant.vue'
import QuadrantsKeyboard from './components/QuadrantsKeyboard.vue'
import words from './assets/words.js'

export default {
  name: 'App',
  components: {
    SingleQuadrant,
    QuadrantsKeyboard
  },
  methods: {
    newGame() {
      this.guesses = []
      this.currentGuess = ''
      this.completed = [false] * 4
      this.answers = []
      this.finalIndices = [0] * 4
      this.topMessage = ''
      this.gameOver = false
      for (let i = 0; i < 4; i++) {
        let newWord = this.randomWord()
        while (this.answers.includes(newWord)) {
          newWord = this.randomWord()
        }
        this.answers.push(newWord)
      }
      console.log("answers: ", this.answers)
    },
    randomWord() {
      // does not work yet, need word list connection
      return words.words[Math.floor(Math.random() * words.words.length)]
    },
    isCurrentGuessWord() {
      return words.words.includes(this.currentGuess) || this.currentGuess.length < 5
    },
    keyPressed(event) {
      if (this.gameOver) {
        return
      }
      if ((this.LOWERCASE.includes(event.key) || this.LOWERCASE.toUpperCase().includes(event.key)) && this.currentGuess.length < 5) {
        this.currentGuess += event.key
      } else if (event.key === this.BACKSPACE_STRING) {
        this.currentGuess = this.currentGuess.slice(0, -1)
      } else if (event.key === this.ENTER_STRING) {
        if (this.currentGuess.length === 5 && words.words.includes(this.currentGuess)) {
          this.guesses.push(this.currentGuess)
          this.completed = this.answers.map(answer => this.guesses.includes(answer))
          this.finalIndices = this.answers.map(answer => this.guesses.indexOf(answer) + 1)
        }
        this.currentGuess = ""
        if (this.completed.filter(complete => complete).length === 4) {
          this.topMessage = "You Win!"
          this.topMessageClass = "success-msg"
          this.gameOver = true
        } else if (this.guesses.length === 9) {
          this.topMessage = "You Lose!"
          this.topMessageClass = "failure-msg"
          this.gameOver = true
        }
      }
    }
  },
  data() {
    return {
      answers: [
        "apple",
        "berry",
        "candy",
        "dates"
      ],
      guesses: [],
      currentGuess: "",
      completed: [
        false,
        false,
        false,
        false,
      ],
      finalIndices: [
        0,
        0,
        0,
        0
      ],
      topMessage: "",
      topMessageClass: "",
      gameOver: false,
    }
  },
  created() {
    this.newGame()
    this.LOWERCASE = "abcdefghijklmnopqrstuvwxyz" // 
    this.ENTER_STRING = "Enter"
    this.BACKSPACE_STRING = "Backspace"

    window.addEventListener('keyup', this.keyPressed)
  },
  mounted() {
    this.$refs["four-wordles"].style.fontSize = this.$refs["four-wordles"].clientHeight / 40 + "px"
    console.log("mounted called, text size set to ", this.$refs["four-wordles"].style.fontSize)
  },
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Nunito&display=swap');



.four-wordles {
  display: grid;
  position: relative;
  top: 0;
  width: 480px;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  justify-content: center;
  background-color: lightblue;
  position: fixed;
  bottom: 160px;
  top: 100px;
}
.new-game {
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  border-radius: 5px;
  cursor: pointer;
  height: 40px;
  width: 80px;
  z-index: 1;
}

.keyboard-container {
  background-color: lightsteelblue;
  position: fixed;
  justify-content: center;
  height: 160px;
  width: 480px;
  bottom: 0;

}

.single-wordle {
  background: lightblue;
  margin: 8px;
}

.header-container {
  position: absolute;
  padding: 8px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  background-color: lightskyblue;
  top: 0;
  right: 0;
  left: 0;
  height: 56px;
  align-items: center;
  font-family: 'Nunito', sans-serif;
}

.main {
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  font-family: 'Nunito', sans-serif;
}

.success-msg {
  color: lightgreen;
}
.failure-msg {
  color: red;
}

.pink-background {
  background-color: pink;
}

.game-over-menu-container {
  position: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  top: 0;
  left: 0;
}

.game-over-menu-box {
  width: 50%;
  height: 50%;
  border-radius: 4rem;
  background-color: blueviolet;
}




</style>

