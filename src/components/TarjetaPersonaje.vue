<template>
  <div>
    <router-link :to="{path: `/${personaje.id}`}">
      <div class="card">
        <div class="card-image">
          <figure class="image is-4by3">
            <img
              :src="personaje.image"
              :alt="personaje.name"
            />
          </figure>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4 has-text-primary-light">{{ personaje.name }}</p>
              <p class="subtitle has-text-weight-bold has-text-primary-light">
                Estado: <span class="subtitle has-text-weight-light has-text-primary-light">{{ estado }}</span>
              </p>
              <p class="subtitle has-text-weight-bold has-text-primary-light block">
                Origen: <span class="subtitle has-text-weight-light has-text-primary-light">{{ origen }}</span>
              </p>
            </div>
          </div>
        </div>
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: "TarjetaPersonaje",
  props: ['personaje'],
  computed: {
    estado() {
      if (this.personaje.status == 'unknown') {
        return 'Desconocido'
      } else {
        return this.personaje.status
      }
    },
    origen() {
      if (this.personaje.location.name == 'unknown') {
        return 'Desconocido'
      } else {
        return this.personaje.location.name
      }
    }
  },
};
</script>

<style>
.card {
  margin: 20px 0;
  height: 450px;
  border: 1px solid gray;
  background-color: transparent !important;
}

.title, .subtitle {
  color: white;
}
</style>