<script setup>
defineProps(["pokemon"]);
defineEmits(["view-details"]);
</script>

<template>
  <div class="bg-gray-100 p-4 rounded-lg shadow-md text-center">
    <p class="font-bold text-lg capitalize">{{ pokemon.name }}</p>
    <img
      :src="pokemon.image"
      alt="Image Pokémon"
      class="w-20 h-20 mx-auto my-4"
    />
    <div class="text-sm text-gray-500">
      <p>Types : {{ pokemon.types.join(", ") }}</p>
    </div>
    <button
      class="bg-teal-300 text-white px-4 py-2 rounded hover:bg-teal-800 mt-2"
      @click="$emit('view-details', pokemon)"
    >
      Voir détails
    </button>
  </div>
</template>
