<script setup>
import { ref } from "vue";

const searchQuery = ref("");
defineEmits(["update:search"]);
</script>

<template>
  <input
    v-model="searchQuery"
    @input="$emit('update:search', searchQuery)"
    type="text"
    placeholder="Rechercher un Pokémon"
    class="border border-gray-300 p-2 w-full rounded"
  />
</template>
