<!--Game-->
<template>
  <h1>Wordle</h1>
  <v-text-field v-model="guess" label="Guess" variant="solo"></v-text-field>
  <v-btn @click="checkGuess">Check</v-btn>
</template>

<script setup lang="ts">
import { reactive, ref } from 'vue'

const guess = ref('')
const wordList = ref(['apple', 'crape', 'peach', 'brick', 'grape', 'chips'])
const secretWord = ref(wordList.value[Math.floor(Math.random() * wordList.value.length)])
const guesses = reactive(new Array<Array<Letter>>())

const checkGuess = () => {
  console.log(guess.value)
  // check length of guess
  if (guess.value.length !== secretWord.value.length) {
    console.log('Guess is the wrong length')
    guess.value = ''
    return
  }

  const secretWordArray = secretWord.value.split('')
  const guessArray = guess.value.split('')

  console.log('-----LOOP-----')
  secretWordArray.forEach((letter, index) => {
    if (letter === guessArray[index]) {
      guessArray[index] = '_'
      console.log(`Correct letter in correct position. Letter: ${letter}`)
    } else if (secretWordArray.includes(guessArray[index])) {
      console.log(`Correct letter in wrong position. Letter: ${guessArray[index]}`)
    } else {
      console.log(`Wrong letter. Letter: ${guessArray[index]}`)
    }
  })
}
</script>
