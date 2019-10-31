<template>
  <div id="app">
    <Header @sendMsg="getMsg" />
    <p v-if="books.length === 0" class="loader"></p>
    <p class="alert alert-warning" v-if="books.length > 0 && filtered.length === 0">No match Found</p>
    <Book @allBooks="getBooks" :filteredArray="filtered" />
  </div>
</template>

<script>
import Header from "./components/Header";
import Book from "./components/Books";

export default {
  name: "app",
  components: {
    Header,
    Book
  },
  data() {
    return {
      search: "",
      language: "",
      filtered: [],
      books: []
    };
  },
  methods: {
    getMsg(msg) {
      this.search = msg[0]; // received search data from header
      this.language = msg[1];
      this.filt();
    },
    getBooks(book) {
      this.books = book; // receive the whole book object from "books"
      this.filt();
    },
    filt() {
      this.filtered = [];
      for (let criteria in this.books) {
        if (this.books[criteria].language.includes(this.language)) {
          if (
            this.books[criteria].description
              .toLowerCase()
              .includes(this.search.toLowerCase()) ||
            this.books[criteria].title
              .toLowerCase()
              .includes(this.search.toLowerCase())
          ) {
            this.filtered.push(this.books[criteria]);
          }
        }
      }
      return this.filtered;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
