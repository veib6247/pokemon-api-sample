<template>
  <!-- BINS -->
  <div class="card w-full bg-base-300 shadow-xl">
    <div class="card-body">
      <!--  -->
      <div class="form-control">
        <label class="label">
          <span class="label-text">Input Pokemon Name</span>
        </label>
        <input
          type="text"
          placeholder="e.g. pikachu"
          class="input input-bordered w-full"
          :class="{ 'input-error': isError }"
          v-model="pokemon" />
      </div>

      <button class="btn" :class="{ loading: isLoading }" @click="getData">
        Get Data
      </button>

      <textarea
        class="textarea textarea-bordered h-96 font-mono"
        spellcheck="false"
        v-if="responseData"
        readonly
        >{{ responseData }}</textarea
      >
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'

  const pokemon = ref('')
  const isLoading = ref(false)
  const isError = ref(false)
  const responseData = ref(null)

  /**
   *
   */
  const getData = async () => {
    isLoading.value = true
    isError.value = false
    responseData.value = null

    try {
      const response = await fetch(
        `https://pokeapi.co/api/v2/pokemon/${pokemon.value}`
      )

      responseData.value = await response.json()
      //
    } catch (error) {
      //
      isError.value = true
      console.error(error)
    } finally {
      //
      isLoading.value = false
    }
  }
</script>
