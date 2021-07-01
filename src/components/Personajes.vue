<template>
  <div class="contenedor">
    <template v-if="this.personajes.length > 0">
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

      <section>
        <b-pagination
          :total="total"
          v-model="current"
          :order="order"
          :per-page="perPage"
          range-before="5"
          range-after="5"
          @change="cambiarPagina(current)"
          class="paginacion"
        >

        </b-pagination>
      </section>
    </template>
    <template v-else>
      <Loader />
    </template>
  </div>
</template>

<script>
import axios from "axios";
import TarjetaPersonaje from "@/components/TarjetaPersonaje.vue";
import Loader from "@/components/Loader.vue";

export default {
  name: "Personajes",
  data() {
    return {
      personajes: [],
      total: 0,
      current: Number(this.$route.query.pagina) || 1,
      order: 'is-centered',
      perPage: 20,
      pagina: Number(this.$route.query.pagina) || 1,
    }
  },
  components: {
    TarjetaPersonaje,
    Loader,
  },
  methods: {
    async getApi(pagina) {
      const url = `https://rickandmortyapi.com/api/character/?page=${pagina}`;
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
          this.total = res.data.info.count
        })
        .catch(error => console.log(error))
    },
    cambiarPagina(pagina) {
      this.$router.push({
        path: this.$route.path,
        query: {
          pagina
        }
      })
    }
  },
  mounted() {
    this.getApi(this.pagina);
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

.pagination-link.is-current {
  background-color: #0d1117 !important;
  border-color: #fff !important;
}

.pagination-link:active {
  border-color: #0d1117 !important;

}

.pagination-previous, .pagination-next, .pagination-link{
  color: #fff !important;
}

.paginacion {
  padding: 10px 0 50px 0;
}
</style>