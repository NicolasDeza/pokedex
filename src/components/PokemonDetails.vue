<script setup>
defineProps(["pokemon"]);
defineEmits(["go-back"]);
</script>

<template>
  <button
    @click="$emit('go-back')"
    class="bg-red-500 text-white px-4 py-2 rounded mb-6 hover:bg-red-600 transition"
  >
    Retour
  </button>

  <div class="flex flex-col justify-center items-center min-h-screen">
    <!-- Carte détaillée -->
    <div class="bg-white shadow-lg rounded-lg overflow-hidden w-80">
      <!-- Image Pokémon -->
      <div
        class="bg-gradient-to-r from-blue-500 to-purple-500 p-6 flex justify-center"
      >
        <img
          :src="pokemon.image"
          alt="Image du Pokémon"
          class="w-32 h-32 object-cover"
        />
      </div>

      <!-- Informations principales -->
      <div class="p-6">
        <!-- Nom -->
        <h2 class="text-2xl font-bold text-gray-800 text-center capitalize">
          {{ pokemon.name }}
        </h2>

        <!-- Types -->
        <div class="flex justify-center gap-2 my-4">
          <span
            v-for="type in pokemon.types"
            :key="type"
            :class="getTypeClass(type)"
            class="px-4 py-1 text-white font-bold rounded-full text-sm"
          >
            {{ type }}
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    getTypeClass(type) {
      const typeClasses = {
        fire: "bg-red-500",
        water: "bg-blue-500",
        grass: "bg-green-500",
        electric: "bg-yellow-500",
        psychic: "bg-pink-500",
        rock: "bg-gray-600",
        ground: "bg-yellow-800",
        ice: "bg-blue-300",
        dark: "bg-gray-800",
        fairy: "bg-pink-300",
        normal: "bg-gray-400",
        fighting: "bg-red-700",
        flying: "bg-blue-300",
        bug: "bg-green-400",
        poison: "bg-purple-500",
        ghost: "bg-purple-700",
        steel: "bg-gray-500",
        dragon: "bg-indigo-500",
      };
      return typeClasses[type] || "bg-gray-500";
    },
  },
};
</script>
