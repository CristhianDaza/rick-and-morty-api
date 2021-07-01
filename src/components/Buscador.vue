<template>
  <b-field class="buscador">
    <b-input
      placeholder="Buscar..."
      v-model="busqueda"
      minlength="3"
      icon="magnify"
      icon-clickable
      type="search"
      @icon-click="buscarPersonaje(busqueda)"
      @keyup.native.enter="buscarPersonaje(busqueda)"
    ></b-input>
  </b-field>
</template>

<script>
import router from '@/router';

export default {
  name: "Buscador",
  data() {
    return {
      busqueda: '',
    }
  },
  methods: {
    buscarPersonaje(busqueda) {
      router.push({
        path: '/',
        query: {
          busqueda,
        }
      })
      this.busqueda = ''
    }
  },
};
</script>

<style>
  .buscador {
    margin-top: 50px;
  }
  .mdi-24px.mdi-set, .mdi-24px.mdi:before{
    color: #7957d5 !important;
  }
</style>