<template>
  <div class="menu-principal">
    <router-link to="/">
        <img :src="Logo" alt="Logo Rick And Morty" />
    </router-link>
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
    padding: 0 50px;
    background-color: #0d1117;
}

.menu-principal img {
    width: 150px;
    padding: 5px 0;
}
</style>