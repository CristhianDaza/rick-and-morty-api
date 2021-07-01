<template>
  <div>
    <Menu />
    <div class="contenedor">
      <template v-if="this.personajeInteresante.length > 0">
        <h1 v-if="personaje !== undefined">{{ personaje.name }}</h1>
        <div class="detallePersonaje">
          <div class="detallePersonaje_imagen">
            <img  v-if="personaje !== undefined" :src="personaje.image" :alt="personaje.name">
          </div>
          <div class="detallePersonaje_info">
            <p><span>Status:</span> {{ personaje.status }}</p>
            <p><span>Gender:</span> {{ personaje.gender }}</p>
            <p><span>Origin:</span> {{ personaje.origin.name }}</p>
            <p><span>Species:</span> {{ personaje.species }}</p>
          </div>
        </div>

        <div class="contenedorEpisodios">
          <h3>Episodios donde apareció</h3>
          <div class="episodios" v-if="this.episode[0].length > 0">
            <div class="episodio" v-for="ep in this.episode[0]" :key="ep.id">
              <p>{{ ep.name }}</p>
              <p>{{ ep.air_date }}</p>
            </div>
          </div>
          <div class="episodios" v-else>
            <div class="episodio" v-for="ep in this.episode" :key="ep.id">
              <p>{{ ep.name }}</p>
              <p>{{ ep.air_date }}</p>
            </div>
          </div>
        </div>

        <div class="personajesInteresante">
          <h2>Personajes interesantes</h2>
          <div class="contenedorPersonajes">
            <div
              v-for="personaje in personajeInteresante[0]"
              :key="personaje.id"
              class="tarjetaPersonajes"
            >
              <TarjetaPersonaje
                :personaje="personaje"
              />
            </div>
          </div>
        </div>
      </template>
      <template v-else>
        <Loader />
      </template>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Menu from '@/components/Menu.vue'
import TarjetaPersonaje from '@/components/TarjetaPersonaje.vue'
import Loader from '@/components/Loader.vue'

export default {
  name: 'personaje',
  data() {
    return {
      personaje: [],
      personajeInteresante: [],
      episodios: [],
      episode: [],
    };
  },
  components: {
    Menu,
    TarjetaPersonaje,
    Loader,
  },
  methods: {
    async getPersonaje() {
      const url = `https://rickandmortyapi.com/api/character/${this.$route.params.id}`;
      const config = {
        methods: 'get',
        url,
        headers: {
          'Content-Type': 'application/json'
        }
      }
      await axios(config)
        .then(res => {
          this.personaje = res.data;
          this.getPersonajeInteresante()

          this.personaje.episode.forEach(ep => {
            this.episodios.push(ep)
          });
          this.getEpisodios(this.episodios)
        })
        .catch(error => console.log(error))
    },
    async getPersonajeInteresante() {
      const url = `https://rickandmortyapi.com/api/character/${this.numeroRandom(1,600)},${this.numeroRandom(1,600)},${this.numeroRandom(1,600)}`;
      const config = {
        methods: 'get',
        url,
        headers: {
          'Content-Type': 'application/json'
        }
      }
      await axios(config)
        .then(res => {
          this.personajeInteresante.push(res.data)
        })
        .catch(error => console.log(error))
    },
    async getEpisodios(urlEp) {
      const episodio = []
      urlEp.forEach(ep => {
        episodio.push(ep.slice(40,42))
      })
      this.getEpisodio(Array(episodio))
    },
    numeroRandom(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
    async getEpisodio(episodio) {
      const url = `https://rickandmortyapi.com/api/episode/${episodio}`;
      const config = {
        methods: 'get',
        url,
        headers: {
          'Content-Type': 'application/json'
        }
      }
      await axios(config)
        .then(res => {
          this.episode.push(res.data)
          console.log(this.episode)
        })
      .catch(error => console.log(error))
    }
  },
  mounted() {
    this.getPersonaje();
  },
}
</script>

<style scoped>
.contenedor h1 {
  font-size: 30px;
  margin: 40px 0;
  font-weight: bold;
}

.detallePersonaje {
  display: flex;
}

.episodios {
  display: flex;
  flex-wrap: wrap;
  margin: 20px 0;
}

.episodio {
  border: 1px solid white;
  padding: 15px;
  margin: 0 15px 15px 0;
}

.episodio p:first-child {
  font-weight: bold;
  margin-bottom: 10px;
}

.detallePersonaje_imagen img {
  width: 300px;
  border-radius: 10px;
}

.detallePersonaje_info {
  padding-left: 35px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
}

.detallePersonaje_info p {
  font-size: 20px;
}

.detallePersonaje_info p span {
  font-weight: bold;
}

.personajesInteresante h2, .contenedorEpisodios h3{
  font-size: 25px;
  margin-top: 50px;
  font-weight: bold;
}
</style>