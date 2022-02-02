<template>
  <v-app dark>
    <v-container>
      <div class="text-center">
        <h1 style="font-size: 25vh">{{ error.statusCode }}</h1>
        <h1>{{ error.message }}</h1>
        <v-btn x-large to="/" class="mt-5">
          <b-icon
            icon="house-fill"
            class="v-icon notranslate v-icon--left"
          ></b-icon>
          Home page
        </v-btn>
      </div>
    </v-container>
  </v-app>
</template>

<script>
export default {
  layout: "empty",
  props: {
    error: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      pageNotFound: "404 Not Found",
      otherError: "An error occurred",
    };
  },
  head() {
    const title =
      this.error.statusCode === 404 ? this.pageNotFound : this.otherError;
    return {
      title,
    };
  },
};
</script>

<style scoped>
h1 {
  font-size: 20px;
}
</style>
