<template>
  <b-field class="buscador">
    <b-input
      placeholder="Buscar..."
      v-model="busqueda"
      type="search"
      icon-pack="fas"
      icon="search"
      minlength="3"
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
</style>