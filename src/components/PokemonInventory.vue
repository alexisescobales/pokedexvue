<template>
    <div class="inventory">
      <!-- Pokéballs -->
      <div class="item" v-for="(item, index) in pokeballs" :key="'pokeball_' + index">
        <img src="/src/assets/pokeball.png" alt="Pokeball" class="icon">
        <div class="details">
          <span class="name">Pokeball</span>
          <span class="quantity">{{ item.quantity }}</span>
        </div>
      </div>
  
      <!-- Pociones -->
      <div class="item" v-for="(item, index) in potions" :key="'potion_' + index">
        <img src="/src/assets/pocion.png" alt="Potion" class="icon">
        <div class="details">
          <span class="name">Poción</span>
          <span class="quantity">{{ item.quantity }}</span>
        </div>
      </div>
  
      <!-- Elixires -->
      <div class="item" v-for="(item, index) in elixirs" :key="'elixir_' + index">
        <img src="/src/assets/elixir.png" alt="Elixir" class="icon">
        <div class="details">
          <span class="name">Elixir</span>
          <span class="quantity">{{ item.quantity }}</span>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      pokeballs: {
        type: Array,
        required: true
      },
      potions: {
        type: Array,
        required: true
      },
      elixirs: {
        type: Array,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
  .inventory {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
  }
  
  .item {
    display: flex;
    align-items: center;
    background-color: #f0f0f0;
    padding: 10px;
    border-radius: 5px;
  }
  
  .icon {
    width: 50px;
    height: 50px;
    margin-right: 10px;
  }
  
  .details {
    display: flex;
    flex-direction: column;
  }
  
  .name {
    font-weight: bold;
  }
  
  .quantity {
    font-size: 0.8em;
  }
  </style>
  