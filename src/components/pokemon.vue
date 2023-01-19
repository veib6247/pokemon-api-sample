<template>
  <div class="flex flex-col gap-3 w-96">
    <!-- input for pokemon name -->
    <div class="form-control">
      <label class="label">
        <span class="label-text">Pokemon Name</span>
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

    <!-- display pokemon info -->
    <div class="card bg-base-300 shadow-xl" v-if="responseData">
      <div class="card-body">
        <h2 class="card-title">{{ responseData.name.toUpperCase() }}</h2>

        <div class="avatar">
          <div class="w-24 rounded-full">
            <img :src="responseData.sprites.front_default" />
          </div>
        </div>

        <div class="prose">
          <h4>Height</h4>
          {{ responseData.height }}
        </div>

        <div class="prose">
          <h4>Weight</h4>
          {{ responseData.weight }}
        </div>
      </div>
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
    // validate input
    if (pokemon.value == '') {
      isError.value = true
      responseData.value = null
    } else {
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
  }
</script>
