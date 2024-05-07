<template>
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
    }
  },
  data() {
    return {
      maxPokeballs: 15, // Definir el máximo de pokeballs permitidas
      maxPotions: 5 // Definir el máximo de pociones permitidas
    };
  },
  methods: {
    buyItem(item) {
      if (item.quantity > 0) {
        // Verificar si se excede el máximo permitido antes de comprar
        if (item.name === 'Pokeball' && item.quantity + 1 > this.maxPokeballs) {
          alert('¡Has alcanzado el máximo de pokeballs permitidas!');
          return;
        }
        if (item.name === 'Poción' && item.quantity + 1 > this.maxPotions) {
          alert('¡Has alcanzado el máximo de pociones permitidas!');
          return;
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


