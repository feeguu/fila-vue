<script setup lang="ts">
import { ref } from 'vue'

// C0001
// C0002

// E0001
// E0002
const queue = ref<string[]>([])
const specialQueue = ref<string[]>([])

const ticketCounter = ref(0)
const specialTicketCounter = ref(0)

const lastCalledTicket = ref('')

const generateTicket = () => {
  ticketCounter.value++
  queue.value.push(`C${ticketCounter.value.toString().padStart(4, '0')}`)
}

const generateSpecialTicket = () => {
  specialTicketCounter.value++
  specialQueue.value.push(`E${specialTicketCounter.value.toString().padStart(4, '0')}`)
}

const callNextTicket = () => {
  lastCalledTicket.value = queue.value.shift() || ''
}

const callNextSpecialTicket = () => {
  lastCalledTicket.value = specialQueue.value.shift() || ''
}
</script>

<template>
  <div class="w-screen h-screen bg-neutral-900 flex flex-col justify-center items-center">
    <main class="w-1/3 h-2/3 flex flex-col justify-between">
      <section class="flex justify-around">
        <button
          class="bg-violet-800 text-white px-4 py-2 rounded-sm w-1/3 h-16"
          v-on:click="generateTicket"
        >
          Gerar senha
        </button>
        <button
          class="bg-violet-800 text-white px-4 py-2 rounded-sm w-1/3 h-16"
          v-on:click="generateSpecialTicket"
        >
          Gerar senha especial
        </button>
      </section>
      <section class="w-full flex justify-around h-full flex-grow-0">
        <div class="w-1/2 flex flex-col items-center">
          <h2 class="text-white text-2xl">Senhas comuns</h2>
          <ul v-if="queue.length !== 0" class="text-white">
            <li v-for="ticket in queue.slice(0, 18)" :key="ticket">{{ ticket }}</li>
          </ul>
          <p v-else class="text-white">Não há senhas comuns</p>
        </div>
        <div class="w-1/2 flex flex-col items-center">
          <h2 class="text-white text-2xl">Senhas especiais</h2>
          <ul v-if="specialQueue.length !== 0" class="text-white">
            <li v-for="ticket in specialQueue.slice(0, 18)" :key="ticket">{{ ticket }}</li>
          </ul>
          <p v-else class="text-white">Não há senhas especiais</p>
        </div>
      </section>
      <section class="flex justify-around">
        <button
          v-on:click="callNextTicket"
          class="bg-violet-800 text-white px-4 py-2 rounded-sm w-1/3 h-16"
        >
          Chamar próxima senha
        </button>
        <button
          v-on:click="callNextSpecialTicket"
          class="bg-violet-800 text-white px-4 py-2 rounded-sm w-1/3 h-16"
        >
          Chamar próxima senha especial
        </button>
      </section>
      <section class="flex justify-center h-12 p-4 flex-grow-0 flex-shrink-0">
        <p v-if="lastCalledTicket !== ''" class="text-white">
          Última senha chamada: <span class="text-violet-400">{{ lastCalledTicket }}</span>
        </p>
      </section>
    </main>
  </div>
</template>
