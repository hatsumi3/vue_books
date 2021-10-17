<template>
  <v-app-bar app color="primary" dark>
    <v-spacer></v-spacer>
    <v-btn color="error" @click="deleteLocalStorage">delete</v-btn>
  </v-app-bar>
</template>

<script>
export default {
  name: "Header",
  methods: {
    deleteLocalStorage() {
      this.$emit("delete-local-storage");
    },
  },
};
</script>

<style></style>
