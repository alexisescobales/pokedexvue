<template>
  <!-- Mostramos el inventario -->
  <div class="shop">
    <h2>Tienda</h2>
    <div class="item" v-for="item in items" :key="item.name">
      <img :src="item.icon" class="icon">
      <div class="details">
        <span class="name">{{ item.name }}</span>
        <div class="quantity-selector">
          <button @click="decreaseQuantity(item)">-</button>
          <input type="number" v-model="item.quantity">
          <button @click="increaseQuantity(item)">+</button>
        </div>
        <button @click="buyItem(item)">Comprar</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true
    },
    itemsInventory: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      maxPokeballs: 15, // Máximo de pokeballs permitidas
      maxPotions: 5 // Máximo de pociones permitidas
    };
  },
  methods: {
    buyItem(item) {
      // Encuentra el ítem correspondiente en el inventario del jugador
      const inventoryItem = this.itemsInventory.find(invItem => invItem.name === item.name);
      if (inventoryItem && item.quantity > 0) {
        switch (item.name) {
          case 'Pokeball':
          case 'Ultraball':
          case 'Masterball':
            if (inventoryItem.quantity + item.quantity > inventoryItem.maxQuantity) {
              alert('¡Has alcanzado el máximo de Pokeballs permitidas en tu inventario!');
              item.quantity = 0;
              return;
            }
            break;
          case 'Poción':
          case 'Elixir':
            if (inventoryItem.quantity + item.quantity > inventoryItem.maxQuantity) {
              alert('¡Has alcanzado el máximo de Pociones permitidas en tu inventario!');
              item.quantity = 0;
              return;
            }
            break;
        }

        // Emitir un evento para indicar que se ha realizado una compra
        this.$emit('buy-item', item);
        // Establecer la cantidad a cero después de la compra
        item.quantity = 0;
      }
    },
    decreaseQuantity(item) {
      if (item.quantity > 0) {
        item.quantity--;
      }
    },
    increaseQuantity(item) {
      // Verificar si se excede el máximo permitido antes de aumentar la cantidad
      if (item.name === 'Pokeball' && item.quantity + 1 > this.maxPokeballs) {
        alert('¡Has alcanzado el máximo de pokeballs permitidas!');
        return;
      }
      if (item.name === 'Poción' && item.quantity + 1 > this.maxPotions) {
        alert('¡Has alcanzado el máximo de pociones permitidas!');
        return;
      }

      if (item.quantity < item.maxQuantity) {
        item.quantity++;
      }
    }
  }
};
</script>

<style scoped>
.icon {
  width: 30px;
  height: 30px;
  margin-right: 10px;
}

.shop {
  position: fixed;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  background-color: rgba(0, 0, 0, 0.8);
  /* Fondo semi-transparente */
  padding: 10px;
  /* Añadir un poco de espacio alrededor del contenido */
  color: whitesmoke;
}
</style>
