<template>
  <div class="article">
    <div class="article-content">
      <div class="apresentation">
        <h2 class="text-apresentation">
          ✨ Start yout reading journey today !
        </h2>
      </div>
      <div class="article-title">
        <h1>Where every page is a new Adventure</h1>
      </div>

      <div class="article-subtitle">
        <h2 class="text">
          Explore a world of knowledge with our extensive collection of free and
          open-source books. Discover compelling stories, learn something new on
          every page, and immerse yourself in the fascinating universe of online
          reading.
        </h2>
      </div>
      <div class="search-bar">
        <input
          type="text"
          placeholder="Search"
          v-model="searchTerm"
          @keyup.enter="searchBook"
        />
      </div>
      <div class="search-results" v-if="book">
        <img :src="book.image" alt="Book cover" />
      </div>
    </div>
  </div>
</template>

<script>
  import axios from "axios";

  export default {
    name: "AppArticle",
    data() {
      return {
        searchTerm: "",
        book: null,
      };
    },
    methods: {
      async searchBook() {
        const response = await axios.get(
          `https://www.googleapis.com/books/v1/volumes?q=${this.searchTerm}&filter=free-ebooks&projection=lite`
        );
        if (response.data.items && response.data.items.length > 0) {
          const bookData = response.data.items[0].volumeInfo;
          this.book = {
            image: bookData.imageLinks ? bookData.imageLinks.thumbnail : "",
            previewLink: bookData.previewLink,
          };
        }
      },
    },
  };
</script>

<style scoped>
  @import url("https://fonts.googleapis.com/css2?family=Handlee&display=swap");

  @import url("https://fonts.googleapis.com/css2?family=Gaegu:wght@300&family=Handlee&display=swap");

  .article-title h1 {
    font-family: "Handlee";
    margin-top: 20px;
    margin-bottom: 20px;
    font-size: 80px;
  }

  .text {
    font-size: 16px;
    font-family: "Gaegu", sans-serif;
  }
  .text-apresentation {
    font-size: 18px;
    font-family: "Gaegu";
    text-align: start;
  }
  input {
    width: 100%;
    height: 3rem;
    padding: 0.5rem;
    font-size: 1.5rem;
    font-family: "Gaegu", sans-serif;
    border-radius: 0.5rem;
    border-style: solid;
    margin-top: 2rem;
  }

  input::placeholder {
    font-family: "Gaegu", sans-serif;
    font-size: 1.5rem;
  }

  .article {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .article-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    width: 65%;
  }

  @media (max-width: 600px) {
    .article-title h1 {
      font-size: 40px;
    }

    .text,
    .text-apresentation {
      font-size: 14px;
    }

    input {
      font-size: 1rem;
    }

    .article {
      width: 100%;
    }

    .article-content {
      width: 100%;
      padding: 1rem;
    }
  }
</style>
