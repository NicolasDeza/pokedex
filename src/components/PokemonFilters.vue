<script setup>
import { ref, onMounted } from "vue";

const types = ref([]);
const selectedType = ref("");
defineEmits(["update:filter"]);

const fetchTypes = async () => {
  const response = await fetch("https://pokeapi.co/api/v2/type/");
  const data = await response.json();
  types.value = data.results.map((type) => type.name);
};

onMounted(fetchTypes);
</script>

<template>
  <select
    v-model="selectedType"
    @change="$emit('update:filter', selectedType)"
    class="border border-gray-300 p-2 rounded w-full"
  >
    <option value="">Tous les types</option>
    <option v-for="type in types" :key="type" :value="type">
      {{ type }}
    </option>
  </select>
</template>
