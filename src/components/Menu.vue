<template>
  <div class="menu-principal">
    <div class="contenedor">
      <router-link to="/">
          <img :src="Logo" alt="Logo Rick And Morty" />
      </router-link>
    </div>
  </div>
</template>

<script>
import Logo from "@/assets/logo.png";

export default {
  name: "Menu",
  data() {
    return {
      Logo,
    };
  },
};
</script>

<style scoped>
.menu-principal {
    background-color: #0d1117;
}

.menu-principal img {
    width: 150px;
    padding: 10px 0 5px 0;
}
</style>