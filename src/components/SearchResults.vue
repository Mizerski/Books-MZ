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
    <div class="grid">
      <div v-for="book in searchedBooks" :key="book.previewLink" class="result">
        <p class="book-title">{{ book.title }}</p>
        <p class="book-author">by {{ book.author }}</p>
        <img :src="book.image" alt="Book cover" />
        <a :href="book.previewLink" target="_blank" class="book-link"
          >Read this book</a
        >
      </div>
    </div>
    <div class="pages">
      <button v-if="startIndex > 0" @click="goBack">
        Load previous results
      </button>
      <button v-if="hasMoreResults" @click="loadMore">Load more results</button>
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
        searchedBooks: [],
        errorMessage: "",
        startIndex: 0,
        maxResults: 10,
        hasMoreResults: false,
      };
    },
    methods: {
      async searchBook() {
        if (!this.searchTerm) {
          this.errorMessage = "Por favor, insira um termo de pesquisa.";
          return;
        }
        const response = await axios.get(
          `https://www.googleapis.com/books/v1/volumes?q=${this.searchTerm}&filter=free-ebooks&projection=lite&startIndex=${this.startIndex}&maxResults=${this.maxResults}`
        );
        if (response.data.items && response.data.items.length > 0) {
          this.searchedBooks = response.data.items.map((item) => {
            const bookData = item.volumeInfo;
            return {
              image: bookData.imageLinks ? bookData.imageLinks.thumbnail : "",
              previewLink: bookData.previewLink,
              title: bookData.title,
              author: bookData.authors ? bookData.authors[0] : "???",
            };
          });
          this.errorMessage = "";
          this.$emit("update", this.searchedBooks.length > 0);
          this.hasMoreResults =
            response.data.totalItems > this.startIndex + this.maxResults;
        } else {
          this.errorMessage = "Nenhum livro encontrado.";
        }
      },
      loadMore() {
        this.startIndex += this.maxResults;
        this.searchBook();
      },
      goBack() {
        this.startIndex = Math.max(0, this.startIndex - this.maxResults);
        this.searchBook();
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
    margin-bottom: 2rem;
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

  .grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 1rem;
  }
  .result img {
    height: 180px;
    object-fit: cover;
  }
  .result {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .book-link {
    font-size: 1.5rem;
    font-family: "Gaegu", sans-serif;
    color: #000;
    text-decoration-color: #000;
  }

  .book-title {
    font-size: 1rem;
    font-family: "Gaegu", sans-serif;
    font-weight: bold;
    color: #000;
  }

  .book-author {
    font-size: 0.8rem;
    font-family: "Gaegu", sans-serif;
    color: #575151;
  }

  .error-message {
    color: red;
    font-size: 1.5rem;
    font-family: "Gaegu", sans-serif;
  }

  .pages {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-top: 2rem;
  }

  button {
    font-size: 1.5rem;
    font-family: "Gaegu", sans-serif;
    color: #000;
    text-decoration-color: #000;
    background-color: #fff;
    border-radius: 0.5rem;
    padding: 0.5rem;
    margin: 0.5rem;
    border-color: #000;
    border-width: 1px;
  }

  @media (max-width: 600px) {
    .grid {
      grid-template-columns: repeat(2, 1fr);
    }
  }

  @media (max-width: 400px) {
    .grid {
      grid-template-columns: 1fr;
    }
  }
</style>
