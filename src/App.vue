<template>
  <div id="app">
    <!-- Componente PokemonList -->
    <!-- Propiedades: pokemons y pokemonTypes -->
    <!-- pokemons: lista de todos los pokémon -->
    <!-- pokemonTypes: lista de tipos de pokémon -->
    <PokemonList :pokemons="pokemons" :pokemonTypes="pokemonTypes" />
  </div>
</template>

<script>
// Importamos el componente PokemonList y la librería axios
import PokemonList from './components/PokemonList.vue';
import axios from 'axios';

export default {
  name: 'App', // Nombre del componente
  components: {
    PokemonList // componente PokemonList
  },
  data() {
    return {
      pokemons: [], // Almacenar todos los pokémon
      pokemonTypes: [] // Almacenar los tipos de pokémon
    };
  },
  mounted() {
    // Método que se ejecuta después de que el componente se ha montado en el DOM
    this.fetchPokemons(); // Llamamos al método para obtener los pokémon
  },
  methods: {
    async fetchPokemons() {
      try {
        // Realizamos una petición GET a la API de PokéAPI para obtener la lista de pokémon
        const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=151');
        const pokemonData = response.data.results; // Extraemos la lista de resultados de la respuesta

        // Iteramos sobre cada pokémon en la lista
        for (let i = 0; i < pokemonData.length; i++) {
          const pokemonResponse = await axios.get(pokemonData[i].url); // Obtenemos los detalles de cada pokémon
          const pokemon = {
            id: pokemonResponse.data.id, // ID del pokémon
            name: this.capitalizeFirstLetter(pokemonResponse.data.name), // Nombre del pokémon con la primera letra en mayúscula
            types: pokemonResponse.data.types.map(type => this.capitalizeFirstLetter(type.type.name)), // Tipos del pokémon
            sprite: pokemonResponse.data.sprites.front_default // Sprite (imagen) del pokémon
          };
          this.pokemons.push(pokemon); // Agregamos el pokémon al array de pokémon
        }

        // Obtenemos y almacenamos los tipos de pokémon
        this.pokemonTypes = this.getAllPokemonTypes();
      } catch (error) {
        console.error('Error fetching Pokémon:', error);
      }
    },
    capitalizeFirstLetter(string) {
      // Función para capitalizar la primera letra de una cadena
      return string.charAt(0).toUpperCase() + string.slice(1);
    },
    getAllPokemonTypes() {
      // Función para obtener todos los tipos de pokémon disponibles
      const types = new Set();
      // Iteramos sobre cada pokémon en la lista
      this.pokemons.forEach(pokemon => {
        // Iteramos sobre cada tipo del pokémon y lo agregamos al conjunto 'types'
        pokemon.types.forEach(type => types.add(type));
      });
      return Array.from(types); // Convertimos el conjunto a un array y lo retornamos
    }
  }
};
</script>

<style>
/* Estilos CSS para el componente */
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
