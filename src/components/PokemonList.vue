<template>
    <!-- Mostrar el equipo -->
    <div class="team-list">
        <h2>Equipo:</h2>
        <ul class="team">
            <li v-for="pokemon in team" :key="pokemon.id">
                <!-- Mostrar la imagen del Pokémon -->
                <img :src="pokemon.sprite" :alt="pokemon.name">
            </li>
        </ul>
    </div>
    <!-- Contenedor principal -->
    <div class="pokemon-list">
        <!-- Añadir cada Pokémon como una card utilizando un bucle v-for -->
        <PokemonCard v-for="pokemon in pokemons" :key="pokemon.id" :pokemon="pokemon" @add-to-team="addToTeam" />
        <!-- Div para mostrar los Pokémon en el equipo -->
    </div>

</template>

<script>
import PokemonCard from './PokemonCard.vue'; // Importa el componente PokemonCard.vue

export default {
    props: {
        pokemons: {
            type: Array,
            required: true
        }
    },
    components: {
        PokemonCard
    },
    data() {
        return {
            team: [] // Inicializa la propiedad team como un array vacío
        };
    },
    methods: {
        addToTeam(pokemon) {
            // Lógica para agregar el Pokémon al equipo
            if (this.team.length < 6 && !this.team.includes(pokemon)) {
                this.team.push(pokemon);
                console.log('Se ha añadido el Pokémon al equipo:', pokemon);
            }
        }
    }
};
</script>

<style scoped>
.pokemon-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  /* Define columnas de ancho flexible */
  gap: 20px;
}

.team-list {
  margin-top: 20px;
  margin-bottom: 20px; /* Añade un margen inferior para separar el equipo del resto de la página */
}

.team-list h2 {
  margin-bottom: 10px;
}


.team-list .team {
    display: inline-block; /* Cambia a visualización en línea para centrar horizontalmente */
    list-style-type: none;
    padding: 0;
    margin: 0;
}

.team-list .team li {
    margin-right: 10px; /* Añade margen derecho entre los elementos */
    margin-bottom: 5px; /* Reduce el margen inferior entre la imagen y el nombre */
    display: inline-block; /* Asegura que los elementos de la lista estén en línea */
}

.team-list img {
  width: 160px; /* Define el tamaño de la imagen */
  height: auto; /* Mantiene la relación de aspecto */
}
</style>