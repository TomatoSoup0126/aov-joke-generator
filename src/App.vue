<template>
  <div class="w-full h-full flex justify-center items-center">
    <div class="max-w-xs w-full flex flex-col">
      <div
        v-if="setup"
        data-qa="setup"
        class="w-3/4 p-4 rounded-2xl bg-teal-800 text-white self-start"
      >
        {{ setup }}
      </div>
      <div
        v-if="delivery && showDelivery"
        data-qa="delivery"
        class="w-3/4 mt-2 p-4 rounded-2xl bg-red-800 text-white self-end"
      >
        {{ delivery }}
      </div>
      <button
        v-if="(!showDelivery && setup)"
        class="bg-green col-span-1 rounded-lg py-2 hover:opacity-90 w-full mx-auto mt-4"
        @click="handleShowDelivery"
      >
        Tell Me!
      </button>
      <button
        v-if="showDelivery"
        class="bg-green col-span-1 rounded-lg py-2 hover:opacity-90 w-full mx-auto mt-4"
        @click="handleShowAnother"
      >
        Another
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const setup = ref('')
const delivery = ref('')
const isLoading = ref(true)
const showDelivery = ref(false)


const getJoke = async () => {
  isLoading.value = true
  try {
    const response = await fetch('https://v2.jokeapi.dev/joke/christmas')
    const data = await response.json()
    setup.value = data.setup
    delivery.value = data.delivery
    isLoading.value = false
  } catch (error) {
    console.log(error)
  }
}

const handleShowDelivery = () => {
  showDelivery.value = true
}

const handleShowAnother = () => {
  showDelivery.value = false
  setup.value = null
  delivery.value = null
  getJoke()
}

getJoke()
</script>
