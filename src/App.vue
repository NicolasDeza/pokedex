<script setup>
import { ref, onMounted } from "vue";
import PokemonList from "./components/PokemonList.vue";
import PokemonDetails from "./components/PokemonDetails.vue";
import SearchBar from "./components/SearchBar.vue";
import PokemonFilters from "./components/PokemonFilters.vue";

const pokemonList = ref([]);
const filteredPokemon = ref([]);
const isLoading = ref(true);
const error = ref(null);
const currentView = ref("list");
const selectedPokemon = ref(null);
const searchQuery = ref("");
const selectedType = ref("");

// Récupérer la liste des Pokémon et leurs types
const fetchPokemonData = async () => {
  try {
    const pokemonResponse = await fetch(
      "https://pokeapi.co/api/v2/pokemon?limit=151"
    );
    const pokemonData = await pokemonResponse.json();

    // Associer les types aux Pokémon
    pokemonList.value = await Promise.all(
      pokemonData.results.map(async (pokemon, index) => {
        const pokemonDetailsResponse = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${index + 1}`
        );
        const pokemonDetails = await pokemonDetailsResponse.json();

        return {
          ...pokemon,
          id: index + 1,
          image: `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${
            index + 1
          }.png`,
          types: pokemonDetails.types.map((type) => type.type.name), // Récupérer les types
        };
      })
    );

    filteredPokemon.value = pokemonList.value;
  } catch (err) {
    err.value = "Erreur lors du chargement des données.";
  } finally {
    isLoading.value = false;
  }
};

// Filtrer les Pokémon en fonction de la recherche et du type
const filterPokemon = () => {
  const query = searchQuery.value.toLowerCase();
  filteredPokemon.value = pokemonList.value.filter((pokemon) => {
    const matchesName = pokemon.name.toLowerCase().includes(query);
    const matchesType =
      !selectedType.value || pokemon.types.includes(selectedType.value);
    return matchesName && matchesType;
  });
};

// Gérer les changements dans la recherche ou les filtres
const updateSearchQuery = (query) => {
  searchQuery.value = query;
  filterPokemon();
};

const updateSelectedType = (type) => {
  selectedType.value = type;
  filterPokemon();
};

// Détail des pokémons
const showDetails = (pokemon) => {
  selectedPokemon.value = pokemon;
  currentView.value = "details";
};

// Revenir à la liste
const showList = () => {
  currentView.value = "list";
};

onMounted(fetchPokemonData);
</script>

<template>
  <div id="app">
    <header class="bg-teal-500 flex justify-center text-white p-4">
      <h1 class="text-xl font-bold">Mon Pokédex</h1>
    </header>

    <main class="container mx-auto p-4">
      <!-- Barre de recherche et filtre par type -->
      <div v-if="currentView === 'list'" class="flex gap-4 mb-4">
        <SearchBar @update:search="updateSearchQuery" />
        <PokemonFilters @update:filter="updateSelectedType" />
      </div>

      <!-- Liste des Pokémon -->
      <PokemonList
        v-if="currentView === 'list'"
        :pokemonList="filteredPokemon"
        :isLoading="isLoading"
        :error="error"
        @view-details="showDetails"
      />

      <!-- Détails du Pokémon -->
      <PokemonDetails v-else :pokemon="selectedPokemon" @go-back="showList" />
    </main>
  </div>
</template>
