<template>
  <div class="contenedor">
    <section class="contenedorPersonajes">
      <div
        v-for="personaje in personajes"
        :key="personaje.id"
        class="tarjetaPersonajes"
      >
        <TarjetaPersonaje
          :personaje=personaje
        />
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
import TarjetaPersonaje from "@/components/TarjetaPersonaje.vue";

export default {
  name: "Personajes",
  data() {
    return {
      personajes: [],
    }
  },
  components: {
    TarjetaPersonaje,
  },
  methods: {
    async getApi() {
      const url = "https://rickandmortyapi.com/api/character";
      const config = {
        methods: 'get',
        url,
        headers: {
          'Content-Type': 'application/json'
        }
      }
      await axios(config)
        .then(res => {
          this.personajes = res.data.results
          console.log(this.personajes)
        })
        .catch(error => console.log(error))
    },
  },
  created() {
    this.getApi();
  },
};
</script>

<style>
.contenedor {
  max-width: 80%;
  margin: 0 auto;
}

.contenedorPersonajes {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
}

.tarjetaPersonajes {
  flex-basis: 24%;
}
</style>