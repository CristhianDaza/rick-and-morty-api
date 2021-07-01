<template>
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
</template>

<script>
import router from '@/router';

export default {
  name: 'filtros',
  data() {
    return {
      gender: this.$route.query.gender || null,
      status: this.$route.query.status || null,
    }
  },
  methods: {
    filtrarPersonajeGender(gender) {
      router.push({
        path: '/',
        query: {
          busqueda: this.$route.query.busqueda,
          gender,
          status: this.$route.query.status,
        }
      })
    },
    filtrarPersonajeStatus(status) {
      router.push({
        path: '/',
        query: {
          busqueda: this.$route.query.busqueda,
          gender: this.$route.query.gender,
          status,
        }
      })
    },
  },
}
</script>

<style>
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
</style>