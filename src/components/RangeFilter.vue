<template>
    <div>
      <label for="min-level">Min:</label>
      <input type="number" id="min-level" v-model="minLevel">
      <label for="max-level">Max:</label>
      <input type="number" id="max-level" v-model="maxLevel">
      <button @click="applyLevelFilter">Filtrar Rango</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        minLevel: 1,
        maxLevel: 151 
      };
    },
    methods: {
      applyLevelFilter() {
        if (this.minLevel <= this.maxLevel) {
          // Emitir un evento con los datos del filtro
          this.$emit('filter', { minLevel: this.minLevel, maxLevel: this.maxLevel });
        } else {
          alert('Minimo no puede ser mayor que el id máximo.');
        }
      }
    }
  };
  </script>
  