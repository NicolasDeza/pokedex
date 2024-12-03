<script setup>
import PokemonCard from "./PokemonCard.vue";
defineProps(["pokemonList", "isLoading", "error"]);
defineEmits(["view-details"]);
</script>

<template>
  <div>
    <div v-if="isLoading" class="text-center text-blue-500 text-lg">
      Chargement des Pokémon...
    </div>
    <div v-else-if="error" class="text-center text-red-500 text-lg">
      {{ error }}
    </div>
    <ul v-else class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-6 p-4">
      <PokemonCard
        v-for="(pokemon, index) in pokemonList"
        :key="index"
        :pokemon="pokemon"
        :index="index"
        @view-details="$emit('view-details', $event)"
      />
    </ul>
  </div>
</template>
