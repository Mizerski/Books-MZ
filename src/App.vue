<template>
  <div id="app">
    <AppHeader />
    <SearchResults :book="book" />
    <div class="content">
      <AppArticle />
      <AppMain />
    </div>
  </div>
</template>

<script>
  import AppHeader from "./components/AppHeader.vue";
  import AppArticle from "./components/AppArticle.vue";
  import AppMain from "./components/AppMain.vue";
  import SearchResults from "./components/SearchResults.vue";
  export default {
    name: "App",
    components: {
      AppHeader,
      AppArticle,
      AppMain,
      SearchResults,
    },
  };
</script>
<style>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    background-color: #f7f9ec;
  }
  #app {
    display: flex;
    flex-direction: column;
    width: 100vw;
    height: 100vh;
  }

  .content {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    height: 100%;
  }
  @media (max-width: 600px) {
    .content {
      flex-direction: column;
    }
  }
</style>
