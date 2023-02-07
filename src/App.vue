<script setup>
import { ref } from 'vue'
import { useStorage } from '@vueuse/core'
import { nanoid } from 'nanoid'
import confetti from 'canvas-confetti'

const newGrocery = ref('')
const groceries = useStorage('groceries', [])

const addGrocery = () => {
  if (newGrocery.value) {
    groceries.value.push({ id: nanoid(), name: newGrocery.value })
    newGrocery.value = ''
  }
}

const deleteGrocery = id => {
  const removeIndex = groceries.value.findIndex(grocery => grocery.id === id)
  groceries.value.splice(removeIndex, 1)
  confetti({ particleCount: 300, spread: 1000, origin: { y: 1} } )
}
</script>

<template>
  <main>
    <h1 class="title">📝 Make a List! (Vue Edition) 📝</h1>
    <form class="newGroceryForm" @submit.prevent="addGrocery">
      <input 
        id="newGrocery" 
        autocomplete="off" 
        type="text" 
        placeholder="Add an item to your list"
        v-model="newGrocery"
      />
      <button type="submit">Add</button>
    </form>
    <h3 class="itemsCounter">Pending Items: {{  groceries.length }}</h3>
    <ul>
      <li v-for="grocery in groceries" @click="deleteGrocery(grocery.id)">
        {{ grocery.name }}
      </li>
    </ul>
  </main>
</template>

<style lang="postcss" scoped>
main {
  @apply flex flex-col justify-center items-center gap-8;
  .title {
    @apply m-2 text-6xl font-light tracking-wider text-blue-800;
  }
  .itemsCounter {
    @apply bg-white w-40 text-center
  }
  form {
    @apply mt-8 flex focus-within:ring-8 focus-within:ring-gray-700 focus-within:rounded-lg;
    input {
      @apply bg-white text-comment p-2 w-80 text-2xl rounded-l-md outline-none;
    }
    button {
      @apply bg-red-800 text-black p-2 text-2xl font-bold rounded-r-md;
      &:hover {
        @apply bg-purple-900;
      }
    }
  }
  ul {
    @apply flex flex-col items-center justify-center rounded-lg bg-gray-700;
    li {
      @apply bg-white text-background m-2 p-2 w-96 text-center;
      &:hover {
        @apply bg-purplish font-bold cursor-pointer;
      }
    }
  }
}
</style>
