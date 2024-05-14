<template>
  <div>
    <div class="pokemon-list-container">
      <!-- Componente Inventario [objeto con los items] -->
      <PokemonInventory :items="inventoryItems" />

      <!-- Componente Tienda [objeto con los shopItems, objeto con los items, funcion buy-item] -->
      <PokemonShop :items="shopItems" :itemsInventory="inventoryItems" @buy-item="buyItemHandler" />
    </div>
    <div>
      <!-- Menú desplegable para seleccionar el tipo de Pokemon -->
      <div class="filter">
        <label for="selected-type">Tipo de Pokemon:</label>
        <select id="selected-type" v-model="selectedType">
          <option value="">Todos los tipos</option>
          <option v-for="type in pokemonTypes" :key="type" :value="type">{{ type }}</option>
        </select>
      </div>

      <!-- Filtro de Pokemons favoritos -->
      <div class="filter">
        <input type="checkbox" id="filter-favorites" v-model="showFavorites">
        <label for="filter-favorites">Pokemons Favoritos</label>
      </div>

      <!-- Filtro de Pokemons del equipo -->
      <div class="filter">
        <input type="checkbox" id="filter-team" v-model="showTeam">
        <label for="filter-team">Pokemons en Equipo</label>
      </div>

      <!-- Agregar el componente RangeFilter -->
      <div class="filter">
        <RangeFilter @filter="handleFilterChange" />
      </div>

      <!-- Lista de Pokemons filtrada -->
      <div class="pokemon-list">
        <!-- Añadir cada Pokemons como card utilizando un bucle v-for -->
        <PokemonCard v-for="pokemon in filteredPokemons" :key="pokemon.id" :pokemon="pokemon" @add-to-team="addToTeam" @add-to-favorites="addToFavorites" @remove-from-team="removeFromTeam" />
        <!-- Div para mostrar los Pokemons en el equipo -->
      </div>
    </div>
  </div>
</template>

<script>
import PokemonInventory from './PokemonInventory.vue'; // Importa el componente de el Inventario
import PokemonShop from './PokemonShop.vue'; // Importa el componente de la tienda
import PokemonCard from './PokemonCard.vue'; // Importa el componente PokemonCard.vue
import RangeFilter from './RangeFilter.vue'; // Importa el componente RangeFilter.vue

export default {
  components: {
    PokemonInventory, // Agrega inventario a los componentes utilizados
    PokemonShop, // Agrega tienda a los componentes utilizados
    PokemonCard, // Agrega PokemonCard a los componentes utilizados
    RangeFilter // Agrega RangeFilter a los componentes utilizados
  },
  props: {
    pokemons: {
      type: Array,
      required: true,
    },
    pokemonTypes: {
      type: Array,
      required: true
    },
  },
  data() {
    return {
      team: [], // Inicializa la propiedad team como array vacío 
      favorite: [], // Lista de Pokemons favoritos
      showFavorites: false, // Para controlar mostrar solo los Pokemon favoritos
      showTeam: false, // Para controlar mostrar solo los Pokemon del equipo
      selectedType: '', // Tipo de Pokemon seleccionado en el menu desplegable
      inventoryItems: [ // Inicializa los datos del inventario
        { name: 'Pokeball', icon: require('../assets/pokeball.png'), quantity: 0, maxQuantity: 15 },
        { name: 'Masterball', icon: require('../assets/masterball.png'), quantity: 0, maxQuantity: 15 },
        { name: 'Ultraball', icon: require('../assets/ultraball.png'), quantity: 0, maxQuantity: 15 },
        { name: 'Poción', icon: require('../assets/pocion.png'), quantity: 0, maxQuantity: 5 },
        { name: 'Elixir', icon: require('../assets/elixir.png'), quantity: 0, maxQuantity: 5 }
      ],
      shopItems: [ // Inicializa los datos de la tienda
        { name: 'Pokeball', icon: require('../assets/pokeball.png'), quantity: 0, maxQuantity: 15 },
        { name: 'Masterball', icon: require('../assets/masterball.png'), quantity: 0, maxQuantity: 15 },
        { name: 'Ultraball', icon: require('../assets/ultraball.png'), quantity: 0, maxQuantity: 15 },
        { name: 'Poción', icon: require('../assets/pocion.png'), quantity: 0, maxQuantity: 5 },
        { name: 'Elixir', icon: require('../assets/elixir.png'), quantity: 0, maxQuantity: 5 }
      ],
      minLevel: 1, // Nivel mínimo de Pokemon
      maxLevel: 151 // Nivel máximo de Pokemon
    };
  },
  computed: {
    filteredPokemons() {
      let filtered = this.pokemons;

      // Segun el tipo seleccionado
      if (this.selectedType) {
        filtered = filtered.filter(pokemon => pokemon.types.includes(this.selectedType));
      }

      // Según los favoritos
      if (this.showFavorites && this.favorite.length >= 1) {
        filtered = filtered.filter(pokemon => this.favorite.includes(pokemon));
      } else if (this.showFavorites && this.favorite.length === 0) {
        alert("No hay pokemons en favoritos");
        filtered = [];
      }

      // Segun el equipo
      if (this.showTeam && this.team.length >= 1) {
        filtered = filtered.filter(pokemon => this.team.includes(pokemon));
      } else if (this.showTeam && this.team.length === 0) {
        alert("No hay pokemons en tu equipo");
        filtered = [];
      }

      // Filtrar por rango de nivel
      filtered = filtered.filter(pokemon => pokemon.id >= this.minLevel && pokemon.id <= this.maxLevel);

      return filtered;
    }
  },
  methods: {
  // Metodo para agregar un Pokemon al equipo
  addToTeam(pokemon) {
    // Verifica si el equipo no tiene ya 6 Pokemon y si el Pokemon no esta ya en el equipo
    if (this.team.length < 6 && !this.team.includes(pokemon)) {
      this.team.push(pokemon);
      // isInTeam del Pokemon como true
      pokemon.isInTeam = true;
      console.log('Se ha anadido el Pokemon al equipo:', pokemon);
    }
  },
  // Metodo para eliminar un Pokemon del equipo
  removeFromTeam(pokemon) {
    // Encuentra el indice del Pokemon en el equipo
    const index = this.team.findIndex(p => p.id === pokemon.id);

    // Si el Pokemon esta en el equipo (indice no es -1), lo elimina
    if (index !== -1) {
      this.team.splice(index, 1);
      // isInTeam del Pokemon como false
      pokemon.isInTeam = false;
      console.log('Se ha eliminado el Pokemon del equipo:', pokemon);
    }
  },
  // Metodo para agregar o eliminar un Pokemon de la lista de favoritos
  addToFavorites(pokemon) {
    // Busca el Pokemon en la lista de favoritos utilizando su id
    const index = this.favorite.findIndex(p => p.id === pokemon.id);
    // Si el Pokemon ya esta en la lista de favoritos, lo elimina
    if (index !== -1) {
      this.favorite.splice(index, 1);
      console.log('Se ha eliminado el Pokemon de favoritos:', pokemon);
    } else { // Si el Pokemon no esta en la lista de favoritos, lo agrega
      this.favorite.push(pokemon);
      console.log('Se ha anadido el Pokemon a favoritos:', pokemon);
    }
  },
  // Manejar la compra de items en la tienda
  buyItemHandler(item) {
    // Busca el item correspondiente en el inventario utilizando su nombre
    const inventoryItem = this.inventoryItems.find(invItem => invItem.name === item.name);
    if (inventoryItem && item.quantity > 0) {
      inventoryItem.quantity += item.quantity;
      item.quantity = 0;
    }
  },
  // Metodo para manejar el cambio en los filtros de nivel
  handleFilterChange(filterData) {
    // Actualiza las variables minLevel y maxLevel
    this.minLevel = filterData.minLevel;
    this.maxLevel = filterData.maxLevel;
  }
}

};
</script>

<style>
/* Estilos para los filtros */
.filter {
  margin-bottom: 10px;
}

.filter label {
  margin-right: 10px;
}

.filter input[type="checkbox"] {
  margin-right: 5px;
}

body{
  background-color: rgb(209, 197, 180);
}
</style>

<style scoped>
/* Estilos específicos del componente */
.icon {
  width: 30px;
  height: 30px;
  margin-right: 10px;
}

.pokemon-list-container {
  display: flex;
  width: 100%;
}

.pokemon-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 20px;
}

.pokemon-list-container {
  display: flex;
}

.team-list {
  margin-top: 20px;
  margin-bottom: 20px;
}

.team-list h2 {
  margin-bottom: 10px;
}

.team-list .team {
  display: inline-block;
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.team-list .team li {
  margin-right: 10px;
  margin-bottom: 5px;
  display: inline-block;
}

.team-list img {
  width: 160px;
  height: auto;
}
</style>
