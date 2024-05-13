<template>
    <div class="pokemon-card">
      <div class="pokemon-image">
        <!-- Sprite del pokemon -->
        <img :src="pokemon.sprite" :alt="pokemon.name"> 
      </div>
      <div class="pokemon-details">
        <!-- Nombre del pokemon -->
        <h3 class="pokemon-name">{{ pokemon.name }}</h3>
        <div class="pokemon-types">
          <!-- Tipo del pokemon -->
          <span v-for="type in pokemon.types" :key="type" class="type">{{ type }}</span>
        </div>
        <!-- ID del pokemon -->
        <p class="pokemon-number">#{{ pokemon.id }}</p>

        <!-- Boton para añadir en equipo (Se mostrara solo si el pokemon no esta en el equipo "!pokemon.isInTeam && !showOnlyTeam") -->
        <i v-show="!pokemon.isInTeam && !showOnlyTeam" @click="addToTeam" class="fa-solid fa-plus" style="font-size: 35px; cursor: pointer; float: right; margin-bottom: 5px;"></i>

        <!-- Boton para añadir eliminar del equipo (Se mostrara solo si el pokemon esta en el equipo "pokemon.isInTeam && !showOnlyTeam") -->
        <i v-show="pokemon.isInTeam && !showOnlyTeam" @click="removeFromTeam" class="fa-solid fa-trash" style="color: #ff0000; font-size: 30px; cursor: pointer; float: right;"></i>

        <!-- Boton para añadir añadir y quitar de favoritos -->
        <i @click="addToFavorites" class="fa-regular" :class="{ 'fa-star fa-solid': isFavorite, 'fa-star': !isFavorite }" style="margin-bottom: 5px; font-size: 30px; cursor: pointer; float: left;"></i>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokemon: {
        type: Object,
        required: true
      },
      showOnlyTeam: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        isFavorite: false // Inicialmente sera false
      };
    },
    methods: {
      addToTeam() {
        // Emitir evento para agregar el Pokemon al equipo
        this.$emit('add-to-team', this.pokemon);
      },
      removeFromTeam() {
        // Emitir evento para eliminar el Pokemon del equipo
        this.$emit('remove-from-team', this.pokemon);
      },
      addToFavorites() {
        // Cambiar el estado de favorito y emitir el evento
        this.isFavorite = !this.isFavorite;
        this.$emit('add-to-favorites', this.pokemon);
      }
    }
  };
  </script>
  
  <style scoped>
  .favorite-icon {
    cursor: pointer;
    margin-bottom: 5px;
  }
  
  .fa-star {
    color: orange;
  }
  
  .pokemon-card {
    border: 1px solid #e2e2e2;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.1);
    background-color: #fff;
  }
  
  .pokemon-image {
    text-align: center;
  }
  
  .pokemon-image img {
    width: 100%;
    height: auto;
  }
  
  .pokemon-details {
    padding: 10px;
  }
  
  .pokemon-name {
    margin: 0;
    font-size: 1.2rem;
  }
  
  .pokemon-types {
    margin-bottom: 5px;
  }
  
  .type {
    display: inline-block;
    padding: 3px 8px;
    border-radius: 4px;
    color: #000;
    font-size: 0.9rem;
  }
  
  .pokemon-number {
    margin: 0;
    font-size: 0.9rem;
    color: #666;
  }
  </style>
  