<script setup>
import {
  computed,
  onBeforeUnmount,
  onMounted,
  ref,
} from 'vue'

import IntroScreen from './components/IntroScreen.vue'
import AnniversaryCountdown from './components/AnniversaryCountdown.vue'
import LoveIntro from './components/LoveIntro.vue'

import OurTimeline from './components/OurTimeline.vue'
import ReasonsILoveYou from './components/ReasonsILoveYou.vue'
import LoveQuestion from './components/LoveQuestion.vue'
import LoveLetter from './components/LoveLetter.vue'
import FinalSurprise from './components/FinalSurprise.vue'

import { relationship } from './data/relationship'

const giftOpened = ref(false)

const now = ref(new Date())

let interval = null

onMounted(() => {
  interval = setInterval(() => {
    now.value = new Date()
  }, 1000)
})

onBeforeUnmount(() => {
  if (interval) {
    clearInterval(interval)
  }
})

const unlockDate = computed(() => {
  return new Date(relationship.unlockAt)
})

const isUnlocked = computed(() => {
  return now.value.getTime() >= unlockDate.value.getTime()
})

// const isUnlocked = computed(() => {
//   // Mientras programas con "npm run dev"
//   // siempre estará desbloqueado.
//   if (import.meta.env.DEV) {
//     return true
//   }


//   return now.value.getTime() >= unlockDate.value.getTime()
// })

const openGift = () => {
  giftOpened.value = true
}
</script>

<template>
  <main class="app">

    <IntroScreen v-if="!giftOpened" @open="openGift" />


    <template v-if="giftOpened">

      <AnniversaryCountdown :is-unlocked="isUnlocked" />


      <template v-if="isUnlocked">
        <LoveIntro />

        <OurTimeline />

        <ReasonsILoveYou />

        <LoveQuestion />

        <LoveLetter />

        <FinalSurprise />

      </template>
    </template>
  </main>
</template>

<style scoped>
.app {
  width: 100%;
  min-height: 100vh;
}
</style>