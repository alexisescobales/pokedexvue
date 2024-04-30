<template>
  <div id="app">
    <PokemonList :pokemons="pokemons" />
  </div>
</template>

<script>
import PokemonList from './components/PokemonList.vue';
import axios from 'axios';

export default {
  name: 'App',
  components: {
    PokemonList
  },
  data() {
    return {
      pokemons: [],
      equipo: []
    };
  },
  mounted() {
    this.fetchPokemons();
  },
  methods: {
  async fetchPokemons() {
    try {
      const response = await axios.get('https://pokeapi.co/api/v2/pokemon?limit=151');
      const pokemonData = response.data.results;
      for (let i = 0; i < pokemonData.length; i++) {
        const pokemonResponse = await axios.get(pokemonData[i].url);
        const pokemon = {
          id: pokemonResponse.data.id,
          name: this.capitalizeFirstLetter(pokemonResponse.data.name),
          types: pokemonResponse.data.types.map(type => this.capitalizeFirstLetter(type.type.name)),
          sprite: pokemonResponse.data.sprites.front_default
        };
        this.pokemons.push(pokemon);
      }
    } catch (error) {
      console.error('Error fetching Pokémon:', error);
    }
  },
  capitalizeFirstLetter(string) {
    return string.charAt(0).toUpperCase() + string.slice(1);
  }
}
};
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
