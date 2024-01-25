<template>
  <div class="main">
    <div class="content">
      <img
        src="../assets/bck.png"
        alt="background image"
        class="background-content"
      />
    </div>
  </div>
</template>

<script>
  export default {
    name: "AppMain",
  };
</script>

<style scoped>
  .main {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 70vh;
    width: 50vw;
    border-radius: 5rem;
  }

  .background-content {
    height: 100%;
    width: auto;
  }

  @media (max-width: 600px) {
    .content {
      height: 40vh;
      width: 100vw;
    }
  }
</style>
