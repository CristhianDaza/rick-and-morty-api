<template>
  <div class="contenedor">
    <template v-if="this.personajes == null">
      <Buscador />
      <p class="noEcontrado">
        Personaje no econtrado
      </p>
    </template>
    <template v-else>
      <template v-if="this.personajes.length > 0">
        <Buscador />
        <div class="filtros">
          <p>Filtrar por:</p>
          <b-field>
            <b-select
              placeholder="Status"
              v-model="status"
              @input="filtrarPersonajeStatus(status)"
            >
              <option value="alive">Alive</option>
              <option value="dead">Dead</option>
              <option value="unknown">Unknown</option>
            </b-select>
            <b-select
              placeholder="Gender"
              v-model="gender"
              @input="filtrarPersonajeGender(gender)"
            >
              <option value="female">Female</option>
              <option value="male">Male</option>
              <option value="genderless">Genderless</option>
              <option value="unknown">Unknown</option>
            </b-select>
          </b-field>
        </div>
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
    </template>
  </div>
</template>

<script>
import axios from "axios";
import TarjetaPersonaje from "@/components/TarjetaPersonaje.vue";
import Buscador from "@/components/Buscador.vue";
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
      busqueda: this.$route.query.busqueda || '',
      status: this.$route.query.status || null,
      gender: this.$route.query.gender || null,
    }
  },
  components: {
    TarjetaPersonaje,
    Loader,
    Buscador,
  },
  methods: {
    async getApi(
      pagina,
      busqueda,
      status,
      gender,
    ) {
      const url = `https://rickandmortyapi.com/api/character/?page=${pagina}&name=${busqueda}&status=${status == null ? '' : status}&gender=${gender == null ? '' : gender}`;
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
        .catch(error => {
          this.personajes = null
          }
        )
    },
    cambiarPagina(pagina) {
      this.$router.push({
        path: this.$route.path,
        query: {
          pagina,
          busqueda: this.$route.query.busqueda,
          status: this.$route.query.status,
          gender: this.$route.query.gender,
        }
      })
    },
    filtrarPersonajeGender(gender) {
      this.$router.push({
        path: this.$route.path,
        query: {
          pagina: this.$route.query.pagina,
          busqueda: this.$route.query.busqueda,
          status: this.$route.query.status,
          gender,
        }
      })
    },
    filtrarPersonajeStatus(status) {
      this.$router.push({
        path: this.$route.path,
        query: {
          pagina: this.$route.query.pagina,
          busqueda: this.$route.query.busqueda,
          status,
          gender: this.$route.query.gender,
        }
      })
    }
  },
  mounted() {
    this.getApi(
      this.pagina,
      this.busqueda,
      this.status,
      this.gender,
    );
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

.noEcontrado {
  font-size: 25px;
}

.filtros{
  display: flex;
  align-items: center;
}

.filtros p {
  font-size: 25px;
}

.filtros .control {
  padding-left: 40px;
  margin: 10px 0;
}

@media (max-width: 768px) {
  .tarjetaPersonajes {
    flex-basis: 33%;
  }
}

@media (max-width: 550px) {
  .tarjetaPersonajes {
    flex-basis: 50%;
  }
  .filtros {
    flex-direction: column;
  }
  .filtros .control {
    padding: 0;
  }
  .contenedor {
    max-width: 90%;
  }
}
@media (max-width: 375px) {
  .tarjetaPersonajes {
    flex-basis: 100%;
  }
}
</style>