<template>
  <header>
    <img src="../assets/web-logo.png" alt="logo" class="header-logo" />
  </header>
</template>

<script>
  export default {
    name: "AppHeader",
  };
</script>

<style scoped>
  header {
    display: flex;
    align-items: flex-start;
    height: 7rem;
  }

  .header-logo {
    height: 100%;
    width: auto;
    margin-left: 2rem;
  }
</style>
