<template>
  <div class="inventory-container">
    <div class="inventory">
      <h1>Inventario</h1>

      <div class="item" v-for="(item, index) in items" :key="index">
        <img :src="item.icon" class="icon">
        <div class="details">
          <span class="name">{{ item.name }}</span>
          <span class="quantity">{{ item.quantity }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true,
      default: () => [
        { name: 'Pokeball', icon: "../assets/pokeball.png", quantity: 10 ,maxQuantity: 15},
        { name: 'Masterball', icon: '../assets/masterball.png', quantity: 7 , maxQuantity: 15 },
        { name: 'Ultraball', icon: '../assets/ultraball.png', quantity: 4, maxQuantity: 15 },
        { name: 'Poción', icon: '../assets/pocion.png', quantity: 2, maxQuantity: 5 },
        { name: 'Elixir', icon: '../assets/elixir.png', quantity: 4, maxQuantity: 5 }
      ]
    }
  }
};
</script>

<style scoped>
.inventory-container {
  position: fixed;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0, 0, 0, 0.8); /* Fondo semi-transparente */
  padding: 10px; /* Añadir un poco de espacio alrededor del contenido */
}

.inventory {
  color: aliceblue;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.item {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.icon {
  width: 30px;
  height: 30px;
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
