<template>
  <div>
    <Menu />
    <div class="contenedor">
      <h1>{{ personaje.name }}</h1>
      <div class="detallePersonaje">
        <div class="detallePersonaje_imagen">
          <img :src="personaje.image" :alt="personaje.name">
        </div>
        <div class="detallePersonaje_info">
          <p><span>Status:</span> {{ personaje.status }}</p>
          <p><span>Gender:</span> {{ personaje.gender }}</p>
          <p><span>Origin:</span> {{ personaje.origin.name }}</p>
          <p><span>Species:</span> {{ personaje.species }}</p>
        </div>
      </div>

      <h2>Episodios donde apareció</h2>
      
      <h2>Personajes interesantes</h2>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Menu from '@/components/Menu.vue'

export default {
  name: 'personaje',
  data() {
    return {
      personaje: [],
    };
  },
  components: {
    Menu,
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
          this.personaje = res.data
          console.log(res.data)
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
</style>