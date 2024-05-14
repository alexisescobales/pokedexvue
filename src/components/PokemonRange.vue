<template>
    <div class="range-filter">
      <label for="level-range-min">Nivel mínimo:</label>
      <input type="range" id="level-range-min" v-model="levelRange.min" :min="minLevel" :max="maxLevel">
      <span>{{ levelRange.min }}</span>
  
      <label for="level-range-max">Nivel máximo:</label>
      <input type="range" id="level-range-max" v-model="levelRange.max" :min="minLevel" :max="maxLevel">
      <span>{{ levelRange.max }}</span>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      minLevel: {
        type: Number,
        default: 1
      },
      maxLevel: {
        type: Number,
        default: 100
      },
      levelRange: {
        type: Object,
        required: true
      }
    }
  };
  </script>
  
  <style scoped>
  .range-filter {
    margin: 10px 0;
  }
  
  .range-filter label {
    margin-right: 10px;
  }
  
  .range-filter input {
    margin-right: 10px;
  }
  </style>
  