<script setup lang="ts">
import { ref, watch } from 'vue'

const question = ref('')
const answer = ref('Questions usually contain a question mark. ;-)')
const loading = ref(false)

// 可以直接侦听一个 ref
watch(question, async (newQuestion, oldQuestion) => {
  if (newQuestion.includes('?')) {
    loading.value = true
    answer.value = 'Thinking...'
    try {
      const res = await fetch('https://yesno.wtf/api')
      answer.value = (await res.json()).answer
    } catch (error) {
      answer.value = 'Error! Could not reach the API. ' + error
    } finally {
      loading.value = false
    }
  }
})

const state = ref(false)
const isRun = ref(false)
function delay(ms: number) {
  return new Promise((resolve) => setTimeout(resolve, ms))
}

// 用来理解await
async function animateElement() {
  state.value = false
  console.log('state', state.value)
  await delay(3000)
  state.value = true
  console.log('state', state.value)
  await delay(3000)
  state.value = false
  console.log('state', state.value)
}

function onBtnClick() {
  isRun.value = true
  console.log('isRun', isRun.value)
  animateElement()
  isRun.value = false
  console.log('isRun', isRun.value)
}
</script>

<template>
  <p>
    Ask a yes/no question:
    <input v-model="question" :disabled="loading" />
  </p>
  <p>{{ answer }}</p>
  <button @click="onBtnClick">Click me</button>
  <div v-if="isRun">Running</div>
  <div v-if="state">I'm animated!</div>
</template>
