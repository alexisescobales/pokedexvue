<template>
    <div>
        <!-- Menú desplegable para seleccionar el tipo de Pokémon -->
        <select v-model="selectedType">
            <option value="">Todos los tipos</option>
            <option v-for="type in pokemonTypes" :key="type" :value="type">{{ type }}</option>
        </select><br>

        <!-- Filtro de Pokémon favoritos -->
        <label for="filter-favorites">Pokemons Favoritos:</label>
        <input type="checkbox" id="filter-favorites" v-model="showFavorites"> <br>

        <!-- Filtro de Pokémon del equipo -->
        <label for="filter-team">Pokemons en Equipo:</label>
        <input type="checkbox" id="filter-team" v-model="showTeam"> <br>

        <!-- Lista de Pokémon filtrada -->
        <div class="pokemon-list">
            <!-- Añadir cada Pokémon como una card utilizando un bucle v-for -->
            <PokemonCard v-for="pokemon in filteredPokemons" :key="pokemon.id" :pokemon="pokemon"
                @add-to-team="addToTeam" @add-to-favorites="addToFavorites" @remove-from-team="removeFromTeam" />
            <!-- Div para mostrar los Pokémon en el equipo -->
        </div>
    </div>
</template>

<script>
import PokemonCard from './PokemonCard.vue'; // Importa el componente PokemonCard.vue

export default {
    props: {
        pokemons: {
            type: Array,
            required: true
        },
        pokemonTypes: {
            type: Array,
            required: true
        }
    },
    components: {
        PokemonCard
    },
    data() {
        return {
            team: [], // Inicializa la propiedad team como un array vacío 
            favorite: [], // Mantén una lista de Pokémon favoritos
            showFavorites: false, // Propiedad para controlar si se muestran solo los Pokémon favoritos
            showTeam: false, // Propiedad para controlar si se muestran solo los Pokémon del equipo
            selectedType: '' // Tipo de Pokémon seleccionado en el menú desplegable
        };
    },
    computed: {
        filteredPokemons() {
            // Filtrar los Pokémon según el tipo seleccionado
            let filtered = this.pokemons;

            if (this.selectedType) {
                filtered = filtered.filter(pokemon => pokemon.types.includes(this.selectedType));
            }

            // Filtrar los Pokémon según los favoritos
            if (this.showFavorites && this.favorite.length >= 1) {
                filtered = filtered.filter(pokemon => this.favorite.includes(pokemon));
            }

            // Filtrar los Pokémon según el equipo
            if (this.showTeam && this.team.length >= 1) {
                filtered = filtered.filter(pokemon => this.team.includes(pokemon));
            }

            return filtered;
        }
    },
    methods: {
        addToTeam(pokemon) {
            // Lógica para agregar el Pokémon al equipo
            if (this.team.length < 6 && !this.team.includes(pokemon)) {
                this.team.push(pokemon);
                pokemon.isInTeam = true; // Establecer isInTeam a true cuando se agrega al equipo
                console.log('Se ha añadido el Pokémon al equipo:', pokemon);
            }
        },
        removeFromTeam(pokemon) {
            // Encuentra el índice del Pokémon en el equipo
            const index = this.team.findIndex(p => p.id === pokemon.id);
            // Si se encuentra el Pokémon en el equipo, elimínalo
            if (index !== -1) {
                this.team.splice(index, 1); // Elimina el Pokémon del array del equipo
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
        }

    }
};
</script>

<style>
body {
    background-color: antiquewhite;
}
</style>

<style scoped>
.pokemon-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    /* Define columnas de ancho flexible */
    gap: 20px;
}

.team-list {
    margin-top: 20px;
    margin-bottom: 20px;
    /* Añade un margen inferior para separar el equipo del resto de la página */
}

.team-list h2 {
    margin-bottom: 10px;
}

.team-list .team {
    display: inline-block;
    /* Cambia a visualización en línea para centrar horizontalmente */
    list-style-type: none;
    padding: 0;
    margin: 0;
}

.team-list .team li {
    margin-right: 10px;
    /* Añade margen derecho entre los elementos */
    margin-bottom: 5px;
    /* Reduce el margen inferior entre la imagen y el nombre */
    display: inline-block;
    /* Asegura que los elementos de la lista estén en línea */
}

.team-list img {
    width: 160px;
    /* Define el tamaño de la imagen */
    height: auto;
    /* Mantiene la relación de aspecto */
}
</style>