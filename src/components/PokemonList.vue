<template>
    <div class="pokemon-list-container">
      <!-- Componente Inventario [objeto con los items] -->
      <PokemonInventory :items="inventoryItems" />

      <!-- Componente Tienda [objeto con los shopItems, objeto con los items, funcion buy-item] -->
      <PokemonShop :items="shopItems" :itemsInventory="inventoryItems" @buy-item="buyItemHandler" />
    </div>
    <div>
      <!-- Menú desplegable para seleccionar el tipo de Pokemon -->
      <select v-model="selectedType">
        <option value="">Todos los tipos</option>
        <option v-for="type in pokemonTypes" :key="type" :value="type">{{ type }}</option>
      </select><br>
  
      <!-- Filtro de Pokemons favoritos -->
      <label for="filter-favorites">Pokemons Favoritos:</label>
      <input type="checkbox" id="filter-favorites" v-model="showFavorites"> <br>
  
      <!-- Filtro de Pokemons del equipo -->
      <label for="filter-team">Pokemons en Equipo:</label>
      <input type="checkbox" id="filter-team" v-model="showTeam"> <br>
  
      <!-- Lista de Pokemons filtrada -->
      <div class="pokemon-list">
        <!-- Añadir cada Pokemons como card utilizando un bucle v-for -->
        <PokemonCard v-for="pokemon in filteredPokemons" :key="pokemon.id" :pokemon="pokemon" @add-to-team="addToTeam" @add-to-favorites="addToFavorites" @remove-from-team="removeFromTeam" />
        <!-- Div para mostrar los Pokemons en el equipo -->
      </div>
    </div>
  </template>
  
  <script>
  import PokemonInventory from './PokemonInventory.vue'; // Importa el componente de el Inventario
  import PokemonShop from './PokemonShop.vue'; // Importa el componente de la tienda
  import PokemonCard from './PokemonCard.vue'; // Importa el componente PokemonCard.vue
  
  export default {
    components: {
      PokemonInventory, // Agrega inventario a los componentes utilizados
      PokemonShop, // Agrega tienda a los componentes utilizados
      PokemonCard // Agrega PokemonCard a los componentes utilizados
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
        ]
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
        if (this.showFavorites && this.favorite.length >= 1) { //Si showFavorites es true y la lista de favoritos es mayor o igual a 1...
          filtered = filtered.filter(pokemon => this.favorite.includes(pokemon)); //Filtered guardara los pokemons favoritos
        } else if (this.showFavorites && this.favorite.length == 0) { //Si es igual a 0...
          // Mostrar la lista vacía + mensaje
          alert("No hay pokemons en favoritos");
          filtered = [];
        }
  
        // Segun el equipo
        if (this.showTeam && this.team.length >= 1) { //Si showTeam es true y la lista de favoritos es mayor o igual a 1...
          filtered = filtered.filter(pokemon => this.team.includes(pokemon)); //Filtered guardara los pokemons de equipo
        } else if (this.showTeam && this.team.length === 0) {//Si es igual a 0...
          // Mostrar la lista vacía + mensaje
          alert("No hay pokemons en tu equipo");
          filtered = [];
        }

        return filtered;
      }
    },
    methods: {
      // Agregar el Pokemon al equipo
      addToTeam(pokemon) {
        if (this.team.length < 6 && !this.team.includes(pokemon)) { //Si el equipo no es mayor de 6 y no esta incluido en la lista
          this.team.push(pokemon); //Se añade
          pokemon.isInTeam = true; // Establecer isInTeam a true cuando se agrega al equipo
          console.log('Se ha añadido el Pokémon al equipo:', pokemon);
        }
      },
      // Eliminar el Pokemon del equipo
      removeFromTeam(pokemon) {
        // Encuentra el índice del Pokémon en el equipo
        const index = this.team.findIndex(p => p.id === pokemon.id);
        // Si se encuentra el Pokémon eliminalo
        if (index !== -1) {
          this.team.splice(index, 1); // Elimina el Pokémon del equipo
          pokemon.isInTeam = false; // Establecer isInTeam a false cuando se elimina del equipo
          console.log('Se ha eliminado el Pokémon del equipo:', pokemon);
        }
      },
      addToFavorites(pokemon) {
        // Busca si el Pokémon ya está en la lista de favoritos
        const index = this.favorite.findIndex(p => p.id === pokemon.id);
  
        if (index !== -1) {
          // Si el Pokémon ya está en la lista de favoritos, elimínalo
          this.favorite.splice(index, 1);
          console.log('Se ha eliminado el Pokémon de favoritos:', pokemon);
        } else {
          // Si el Pokémon no está en la lista de favoritos, agrégalo
          this.favorite.push(pokemon);
          console.log('Se ha añadido el Pokémon a favoritos:', pokemon);
        }
      },
      // Maneja la compra de objetos desde la tienda y actualizar el inventario del jugador
      buyItemHandler(item) {
        // Encuentra el item correspondiente en el inventario del jugador
        const inventoryItem = this.inventoryItems.find(invItem => invItem.name === item.name);
        if (inventoryItem && item.quantity > 0) {
          // Actualiza la cantidad en el inventario del jugador
          inventoryItem.quantity += item.quantity;
          // Establece la cantidad del item comprado en 0 en la tienda
          item.quantity = 0;
        }
      },
    }
  };
  </script>

<style>
body {
    background-color: antiquewhite;
}
</style>

<style scoped>

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
