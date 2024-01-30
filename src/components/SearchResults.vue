<template>
  <div class="search-results">
    <div class="search-bar">
      <input
        type="text"
        placeholder="Search"
        v-model="searchTerm"
        @keyup.enter="searchBook"
      />
      <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
    </div>
    <div v-if="searchedBook" class="result">
      <img :src="searchedBook.image" alt="Book cover" />
    </div>
  </div>
</template>

<script>
  import axios from "axios";
  export default {
    name: "SearchResults",
    props: {
      book: Object,
    },
    data() {
      return {
        searchTerm: "",
        searchedBook: null,
        errorMessage: "",
      };
    },
    methods: {
      async searchBook() {
        if (!this.searchTerm) {
          this.errorMessage = "Por favor, insira um termo de pesquisa.";
          return;
        }
        const response = await axios.get(
          `https://www.googleapis.com/books/v1/volumes?q=${this.searchTerm}&filter=free-ebooks&projection=lite`
        );
        if (response.data.items && response.data.items.length > 0) {
          const bookData = response.data.items[0].volumeInfo;
          this.searchedBook = {
            image: bookData.imageLinks ? bookData.imageLinks.thumbnail : "",
            previewLink: bookData.previewLink,
          };
          this.errorMessage = "";
        } else {
          this.errorMessage = "Nenhum livro encontrado.";
        }
      },
    },
  };
</script>

<style scoped>
  .search-bar {
    display: flex;
    width: 100%;
    padding-right: 1rem;
    padding-left: 1rem;
    flex-direction: column;
  }
  input {
    display: flex;
    width: 100%;
    padding: 0.5rem;
    font-size: 1.5rem;
    font-family: "Gaegu", sans-serif;
    border-radius: 0.5rem;
    border-style: solid;
  }

  .result {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 2rem;
  }

  .error-message {
    color: red;
    font-size: 1.5rem;
    font-family: "Gaegu", sans-serif;
  }
</style>
