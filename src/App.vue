<script setup lang="ts">
import { ref, type Ref } from 'vue'

const isListening: Ref<boolean> = ref<boolean>(false)
const interimTranscript: Ref<string> = ref<string>('')
const finalTranscript: Ref<string> = ref<string>('')
const error: Ref<string> = ref<string>('')
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition

const recognition = new SpeechRecognition()
recognition.continuous = true
recognition.interimResults = true
recognition.lang = 'uk-UA'
recognition.maxAlternatives = 10

const setIsListening = (state: boolean) => (isListening.value = state)

recognition.onstart = () => {
  console.log('onstart')
}

recognition.onend = () => {
  console.log('onend')
  recognition.start()
}

recognition.onaudiostart = () => {
  console.log('onaudiostart')
}

recognition.onaudioend = () => {
  console.log('onaudioend')
}

recognition.onsoundstart = () => {
  console.log('onsoundstart')
}

recognition.onsoundend = () => {
  console.log('onsoundend')
}

recognition.onspeechstart = () => {
  setIsListening(true)
  console.log('onspeechstart')
}

recognition.onspeechend = () => {
  setIsListening(false)
  console.log('onspeechend')
}

recognition.onresult = (event) => {
  console.log('onresult', event)

  for (let i = event.resultIndex; i < event.results.length; ++i) {
    if (event.results[i].isFinal) {
      finalTranscript.value += event.results[i][0].transcript
    } else {
      interimTranscript.value += event.results[i][0].transcript
    }
  }

  console.log('finalTranscript', finalTranscript.value)
  console.log('interimTranscript', interimTranscript.value)
}

recognition.onerror = () => {
  console.log('onerror')
  error.value = 'Error'
}

recognition.start()
</script>

<template>
  <p v-if="isListening">I'm listening...</p>
  <p v-if="error">{{ error }}</p>
  <p>finalTranscript: {{ finalTranscript }}</p>
  <p>interimTranscripts: {{ interimTranscript }}</p>
</template>
