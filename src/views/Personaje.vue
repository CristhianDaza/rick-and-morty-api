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

        <h2>Episodios donde apareció</h2>

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
          this.getEpisodio(this.episodios)
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
    async getEpisodio(urlEp) {
      const url = `${urlEp}`;
      const config = {
        methods: 'get',
        url,
        headers: {
          'Content-Type': 'application/json'
        }
      }
      await axios(config)
        .then(res => {
          this.episode = res.data
          console.log(this.episode)
        })
        .catch(error => console.log(error))
    },
    numeroRandom(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
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

.detallePersonaje_imagen img {
  width: 200px;
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

.personajesInteresante h2{
  font-size: 20px;
}
</style>