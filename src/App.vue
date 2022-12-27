<template>
  <div class="h-full flex justify-center items-center">
    <div class="p-10 shadow-lg shadow-orange-200 border-4 border-orange-200">
      <h1 class="text-center text-blue-400 text-4xl mb-10">
        <span class="block">{{ setYear }}</span
        >Christmas Countdown
      </h1>
      <ul class="flex items-center justify-around">
        <li class="text-center">
          <p class="text-red-300 text-xl">{{ reciprocal.day }}</p>
          <p class="text-orange-600">Days</p>
        </li>
        <li class="text-center">
          <p class="text-red-300 text-xl">{{ reciprocal.hour }}</p>
          <p class="text-orange-600">Hours</p>
        </li>
        <li class="text-center">
          <p class="text-red-300 text-xl">{{ reciprocal.min }}</p>
          <p class="text-orange-600">Minutes</p>
        </li>
        <li class="text-center">
          <p class="text-red-300 text-xl">{{ reciprocal.sec }}</p>
          <p class="text-orange-600">seconds</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted } from 'vue'
const today = new Date()
const setMouthDay = '/12/25'
const setYear =
  new Date(today.getFullYear() + setMouthDay).getTime() - today.getTime() > 0
    ? today.getFullYear()
    : today.getFullYear() + 1

const timestamp = {
  christmasDayTime: new Date(setYear + setMouthDay).getTime(),
  todayTime: today.getTime(),
  dayTime: 1000 * 60 * 60 * 24,
  hoursTime: 1000 * 60 * 60,
  minTime: 1000 * 60,
  secTime: 1000,
}

const reciprocal = reactive({})

const contdownDay = () => {
  reciprocal.day = Math.floor(reciprocal.gap / timestamp.dayTime)
  timestamp.dayTimeTamp = timestamp.dayTime * reciprocal.day

  reciprocal.hour = Math.floor((reciprocal.gap - timestamp.dayTimeTamp) / timestamp.hoursTime)
  timestamp.hourTimeTamp = timestamp.hoursTime * reciprocal.hour

  reciprocal.min = Math.floor((reciprocal.gap - timestamp.dayTimeTamp - timestamp.hourTimeTamp) / timestamp.minTime)
  timestamp.minTimeTamp = timestamp.minTime * reciprocal.min

  reciprocal.sec = Math.floor(
    (reciprocal.gap - timestamp.dayTimeTamp - timestamp.hourTimeTamp - timestamp.minTimeTamp) / timestamp.secTime
  )
}

const contdownTimer = setInterval(() => {
  reciprocal.gap -= 1000
  contdownDay()
}, 1000)

onMounted(() => {
  reciprocal.gap = timestamp.christmasDayTime - timestamp.todayTime
  contdownDay()
  contdownTimer
})
</script>
