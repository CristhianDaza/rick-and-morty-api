<template>
  <div>
    <TarjetaPersonaje />
  </div>
</template>

<script>
import axios from "axios";
import TarjetaPersonaje from "@/components/TarjetaPersonaje.vue";

export default {
  name: "Personajes",
  components: {
    TarjetaPersonaje,
  },
  methods: {
    async getApi() {
      try {
        const api = "https://rickandmortyapi.com/api/character";
        const fetch = await axios.get(api);
        const data = await fetch.data;
        console.log(data);
      } catch (error) {
        console.log(error);
      }
    },
  },
  created() {
    this.getApi();
  },
};
</script>