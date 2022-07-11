<template>
  <div class="flex items-center flex-col gap-6 w-full h-screen justify-center">

    <span @click="test" class="absolute top-10 text-[150px] opacity-40" > {{ countdown }} </span>
    
    <!-- Word list Box -->
    <div id="wordBox" class="w-[35%] flex items-center justify-center bg-gray-200 flex-wrap bg-opacity-50 rounded shadow" >
      
      <!-- Word Box -->
      <div id="word" 
        v-for="word in list" 
        :key="word" 
        :class="{ trueW: word.typedState == true, wrong: word.typedState == false, selectMode: word.isSelected == true }" 
        class="m-2 p-2 rounded font-semibold bg-gray-200"
      >
        {{ word.text }}
      </div>

    </div>

    <!-- Input Box -->
    <input 
    v-model="currentType" 
    @input="start" 
    @keyup.space="skipWord" 
    class="w-[35%] h-12 rounded bg-gray-200 bg-opacity-50 shadow p-3 outline-none focus-within:shadow-black transition-all" 
    placeholder="Type Here" 
    type="text"
    autofocus>

    <!-- Restart Button -->
    <button @click="restart" > <img class="opacity-90" src="../public/retry.svg" alt=""> </button>

    <!-- Score Table -->
    <div v-if="gameEnd" class="w-[35%] flex items-center justify-center absolute bottom-10 bg-gray-200 bg-opacity-50 rounded shadow" >
      <span class="text-green-600 text-xl" ><strong > {{ result.correct }} </strong> Correct Word</span>
    </div>

  </div>
</template>

<script setup>

import { onMounted, ref, watch } from 'vue'
import randomWords from 'random-words';

const gameEnd = ref(false)
const list = ref(null)
const countdown = ref(60)
const currentType = ref(undefined) // User input
const result = ref({
  correct: 0,
  wrong: 0
})

let currentWord;
let timer;
let startState = false

const start = () => {
  if (startState) return
  if (list.value.length == 0) return

  startState = true 
  timer = setInterval(() => {
    countdown.value -= 1
  if (countdown.value <= 0) {
    clearInterval(timer)
    startState = false
    list.value = []
    countdown.value = 60
    gameEnd.value = true
    currentType.value = ''
  }
  }, 1000);
}

const restart = () => {
  clearInterval(timer)
  startState = false
  createList()
  countdown.value = 60
  currentType.value = ''
  gameEnd.value = false
  result.value = {
    correct: 0,
    wrong: 0
  }
}

const skipWord = (word, state) => {

  if (!list.value.length) return
  
  let index = getIndex()
  if (currentType.value.trimEnd() !== currentWord.text) {
    currentWord.typedState = false
    result.value.wrong++
  } else {
    result.value.correct++
  }
  // If this is the last word in the list then reload the list
  if (!list.value[index + 1]) {
    createList()
  } else {
    currentWord.isSelected = false
    list.value[index + 1].isSelected = true
    currentWord = list.value[index + 1]
  }

  currentType.value = ''
}

// Check real-time current word is correct or not
watch(currentType, () => {

  if (!currentWord.text.includes(currentType.value.trimEnd())) {
    currentWord.typedState = false
  } else {
    currentWord.typedState = null
  }

  if (currentType.value.trimEnd() == currentWord.text) {
    currentWord.typedState = true
  }

})

const getIndex = () => {
  let index = list.value.findIndex(w => w?.isSelected == true)
  return index
}

const createList = () => {
  list.value = randomWords(15).map(word => {
    return {
      text: word,
      isSelected: false,
      typedState: null
    }
  })
  currentWord = list.value[0]
  currentWord.isSelected = true
  return list.value
}

onMounted(() => {
  createList()
})

</script>

<style>

.selectMode {
  background-color: #f9fafb !important;
}

.trueW {
  background-color: #22c55e !important;
}

.wrong {
  background-color: #ef4444 !important;
}

</style>