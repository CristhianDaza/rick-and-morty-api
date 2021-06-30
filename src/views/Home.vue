<template>
  <div>
    <Menu />
    <Personajes />
  </div>
</template>

<script>
import Menu from '@/components/Menu.vue'
import Personajes from '@/components/Personajes.vue'

export default {
  name: "Home",
  components: {
    Menu,
    Personajes,
  },
};
</script>