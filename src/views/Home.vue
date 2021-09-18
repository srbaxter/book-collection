<template>
  <section class="search-section">
    <h1>Are you ready to discover new books?</h1>
    <form class="search-form" @submit.prevent="submitSearch">
      <label for="search">Search for a book title:</label><br />
      <input
        type="text"
        name="search"
        id="search"
        class="search"
        v-model.trim="search"
      />
      <button type="submit" class="search-button">Search</button>
    </form>
  </section>

  <section class="search-results">
    <div v-if="isLoading" class="results-message">
      Your results are still loading...
    </div>
    <div v-else-if="!isLoading">
      <div v-if="books.length" class="book-list">
        <div
          v-for="book in books"
          :key="book.key"
          class="card"
          @click="viewBook(book)"
        >
          <img :src="book.cover_link" :alt="book.title" />
          <div class="book-title">{{ book.title }}</div>
          <div
            v-for="(auth, index) in book.author_name"
            :key="auth"
            class="book-author"
          >
            {{ auth }}<span v-if="index + 1 < book.author_name.length">, </span>
          </div>
        </div>
      </div>
      <div v-else-if="!isLoading && sentSearch" class="results-message">
        No results found...
      </div>
    </div>
  </section>
</template>

<script>
import { computed, reactive, toRefs } from "vue";
import { useRouter } from "vue-router";
export default {
  setup() {
    const base_url = "https://openlibrary.org/";
    const photo_url = "https://covers.openlibrary.org/";
    const router = useRouter();
    const state = reactive({
      bookList: [],
      search: "",
      isLoading: null,
      sentSearch: null,
      // take the list of books and add a link for each book image
      books: computed(() => {
        const b = state.bookList;

        return b.map((book) => {
          if (book.cover_edition_key && book.cover_edition_key.trim().length) {
            book.cover_link = `${photo_url}b/olid/${book.cover_edition_key}-L.jpg`;
          } else {
            book.cover_link = require("@/assets/default-cover.jpg");
          }
          return book;
        });
      }),
    });

    // search for a book title at OpenLibrary
    const submitSearch = async () => {
      state.isLoading = true;
      if (state.search.length >= 3) {
        state.sentSearch = true;
        const res = await fetch(`${base_url}search.json?title=${state.search}`);
        const data = await res.json();
        state.bookList = { ...data }.docs;
      }
      state.isLoading = false;
    };

    // we picked a book. get the main book ID hopefully of the cover photo
    // send us to the Book page
    function viewBook(picked) {
      console.log(picked);
      const { cover_edition_key: cover, seed: editions } = picked;
      let bookID = "";

      if (cover && cover.trim().length) {
        bookID = cover;
      } else {
        if (editions.length && editions[1]) {
          bookID = editions[1].split("/").pop();
        } else if (editions.length) {
          bookID = editions[0].split("/").pop();
        }
      }
      router.push({
        name: "Book",
        params: {
          id: bookID,
        },
      });
    }

    return { submitSearch, ...toRefs(state), viewBook };
  },
};
</script>

<style scoped>
.search-section {
  text-align: center;
  width: 80%;
  max-width: 90%;
  margin: 0 auto 10px auto;
  padding: 50px 0;
}
h1 {
  margin-bottom: 50px;
  font-size: 1.8em;
}
label {
  font-weight: bold;
}
.search {
  padding: 10px;
  font-size: 16px;
  border-radius: 10px;
  width: 75%;
  max-width: 400px;
}
.search-button {
  font-size: 16px;
  padding: 10px;
  background-color: blue;
  border-color: blue;
  color: white;
  border-radius: 10px;
  margin-left: 2px;
  cursor: pointer;
}

.search-results {
  margin-bottom: 50px;
}
.book-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  grid-gap: 15px;
  width: 95%;
  max-width: 95%;
  margin: auto;
}

.card {
  background-color: rgb(221, 219, 207);
  border-color: rgb(221, 219, 207);
  color: black;
  display: flex;
  flex-direction: column;
  padding: 10px;
  border-radius: 10px;
  height: 450px;
  cursor: pointer;
}
.card img {
  height: 75%;
  max-height: 75%;
  max-width: 100%;
  margin-bottom: 25px;
}
.results-message {
  text-align: center;
}
.book-title {
  font-weight: bold;
  font-size: 1.4em;
  margin: 5px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  width: 100%;
}
.book-author {
  margin: 5px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  width: 100%;
}
</style>
