<template>
  <div>
    <router-view :key="$route.fullPath" />
  </div>
</template>

<script>
export default {
}
</script>

<style>
  * {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
    color: white;
  }
  body {
    background: #1c2128;
    min-height: 100vh;
  }
</style>
