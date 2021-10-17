<template>
  <v-app>
    <Header @delete-local-storage="deleteLogalStorage" />
    <v-main>
      <v-container>
        <router-view
          @add-book-list="addBook"
          @update-book-info="updateBookInfo"
          :books="books"
        />
      </v-container>
    </v-main>
    <Footer />
  </v-app>
</template>

<script>
import Header from "@/global/Header.vue";
import Footer from "@/global/Footer.vue";
const STORAGE_KEY = "books";
export default {
  name: "App",

  data: () => ({
    books: [],
    newBook: null,
  }),
  components: {
    Header,
    Footer,
  },
  mounted() {
    if (localStorage.getItem(STORAGE_KEY)) {
      try {
        this.books = JSON.parse(localStorage.getItem(STORAGE_KEY));
      } catch (e) {
        localStorage.removeItem(STORAGE_KEY);
      }
    }
  },
  methods: {
    addBook(e) {
      this.books.push({
        id: this.books.length,
        title: e.title,
        image: e.image,
        description: e.description,
        readData: "",
        memo: "",
      });
      this.saveBooks();
      console.log(this.books.slice(-1)[0].id);
      this.goToEditPage(this.books.slice(-1)[0].id);
    },
    removeBook(index) {
      this.books.splice(index, 1);
      this.saveBooks();
    },
    saveBooks() {
      const parsed = JSON.stringify(this.books);
      localStorage.setItem(STORAGE_KEY, parsed);
    },
    updateBookInfo(e) {
      const updateInfo = {
        id: e.id,
        readDate: e.readDate,
        memo: e.memo,
        title: this.books[e.id].title,
        image: this.books[e.id].image,
        description: this.books[e.id].description,
      };
      this.books.splice(e.id, 1, updateInfo);
      this.saveBooks();
      this.$router.push(`/`);
    },
    deleteLogalStorage() {
      const isDeleted = "delete all items. are you sure?";
      if (window.confirm(isDeleted)) {
        localStorage.setItem(STORAGE_KEY, "");
        localStorage.removeItem(STORAGE_KEY);
        this.books = [];
        window.location.reload;
      }
    },
    goToEditPage(id) {
      this.$router.push(`/edit/${id}`);
    },
  },
};
</script>
