<template>
    <div class="pokemon-card">
        <div class="pokemon-image">
            <img :src="pokemon.sprite" :alt="pokemon.name">
        </div>
        <div class="pokemon-details">
            <h3 class="pokemon-name">{{ pokemon.name }}</h3>
            <div class="pokemon-types">
                <span v-for="type in pokemon.types" :key="type" class="type">{{ type }}</span>
            </div>
            <p class="pokemon-number">#{{ pokemon.id }}</p>
            <i @click="addToTeam" class="fa-solid fa-plus" style="font-size: 40px; cursor: pointer; float: right;"></i>
            <i @click="addToFavoites" class="fa-regular fa-star" style="font-size: 30px; cursor: pointer; float: left;"></i>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        pokemon: {
            type: Object,
            required: true
        }
    },
    methods: {
        addToTeam() {
            // Emitir evento para agregar el Pokémon al equipo
            this.$emit('add-to-team', this.pokemon);
        }
    }
};
</script>

<style scoped>
.favorite-icon {
    cursor: pointer;
    /* Cambia el cursor al pasar sobre el icono */
    margin-bottom: 5px;
    /* Espaciado inferior */
}

.fa-star {
    color: orange;
    /* Color amarillo para indicar que es favorito */
}

.pokemon-card {
    border: 1px solid #e2e2e2;
    /* Borde de la tarjeta */
    border-radius: 8px;
    /* Borde redondeado */
    overflow: hidden;
    /* Oculta el desbordamiento */
    box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.1);
    /* Sombra */
    background-color: #fff;
    /* Fondo blanco */
}

.pokemon-image {
    text-align: center;
    /* Centra la imagen */
}

.pokemon-image img {
    width: 100%;
    /* Imagen ocupa todo el ancho */
    height: auto;
    /* Altura automática */
}

.pokemon-details {
    padding: 10px;
    /* Espaciado interno */
}

.pokemon-name {
    margin: 0;
    /* Margen cero */
    font-size: 1.2rem;
    /* Tamaño de fuente */
}

.pokemon-types {
    margin-bottom: 5px;
    /* Espaciado inferior */
}

.type {
    display: inline-block;
    /* Elementos en línea */
    padding: 3px 8px;
    /* Relleno */
    border-radius: 4px;
    /* Borde redondeado */
    color: #000;
    /* Color de texto (negro) */
    font-size: 0.9rem;
    /* Tamaño de fuente */
}

.pokemon-number {
    margin: 0;
    /* Margen cero */
    font-size: 0.9rem;
    /* Tamaño de fuente */
    color: #666;
    /* Color de texto */
}
</style>