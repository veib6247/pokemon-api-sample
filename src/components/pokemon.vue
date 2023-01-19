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

<template>
  <div class="flex w-96 flex-col gap-3">
    <div class="prose mb-9 text-center">
      <h1 class="text-accent">Pokémon API</h1>
      <p>Sample Implementation</p>
    </div>

    <!-- input for pokemon name -->
    <div class="form-control">
      <input
        type="text"
        placeholder="Pokemon Name or ID"
        class="input-bordered input-primary input w-full text-center placeholder-gray-700"
        :class="{ 'input-error': isError }"
        @keyup.enter="getData"
        v-model="pokemon"
        spellcheck="false" />
    </div>

    <button
      class="btn-primary btn gap-2"
      :class="{ loading: isLoading }"
      @click="getData">
      <svg
        v-if="!isLoading"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        stroke-width="1.5"
        stroke="currentColor"
        class="h-6 w-6">
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          d="M21 21l-5.197-5.197m0 0A7.5 7.5 0 105.196 5.196a7.5 7.5 0 0010.607 10.607z" />
      </svg>
      Search
    </button>

    <!-- display pokemon info -->
    <Transition>
      <div class="card mt-3 bg-base-300 shadow-xl" v-if="responseData">
        <div class="card-body items-center text-center">
          <h2 class="card-title text-secondary">
            {{ responseData.name.toUpperCase() }}
          </h2>

          <div class="avatar">
            <div class="w-24 rounded-full">
              <img :src="responseData.sprites.front_default" />
            </div>
          </div>

          <div class="prose">
            <h4 class="text-secondary">ID</h4>
            <span class="font-mono">
              {{ responseData.id }}
            </span>
          </div>

          <div class="prose">
            <h4 class="text-secondary">Height</h4>
            <span class="font-mono">
              {{ responseData.height }}
            </span>
          </div>

          <div class="prose">
            <h4 class="text-secondary">Weight</h4>
            <span class="font-mono">
              {{ responseData.weight }}
            </span>
          </div>

          <div class="prose">
            <h4 class="text-secondary">Base Experience</h4>
            <span class="font-mono">
              {{ responseData.base_experience }}
            </span>
          </div>
        </div>
      </div>
    </Transition>

    <div class="text-center">
      <p class="text-sm">
        <a
          class="link"
          href="https://github.com/veib6247/fetching-data-samples"
          target="_blank">
          Github
        </a>
      </p>
    </div>
  </div>
</template>

<style scoped>
  .v-enter-active,
  .v-leave-active {
    transition: opacity 0.5s ease;
  }

  .v-enter-from,
  .v-leave-to {
    opacity: 0;
  }
</style>
