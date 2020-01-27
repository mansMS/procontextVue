<template>
  <Images
    v-bind:images="images"
    v-bind:albumId="albumId"
    v-bind:imageId="imageId"
    v-bind:loading="loading"
    v-on:select-image="$emit('select-image', $event)"
  />
</template>

<script>
import Images from "../components/Images.vue";

export default {
  name: "ImagesContainer",

  props: ["albumId", "imageId"],

  data() {
    return {
      imagesList: {},
      images: [],
      loading: false
    };
  },

  watch: {
    albumId(id) {
      this.images = [];
      this.fetchData(id);
    }
  },

  methods: {
    fetchData: async function(albumId) {
      if (albumId in this.imagesList) {
        this.images = this.imagesList[albumId];
      } else {
        this.loading = true;

        const axios = require("axios");

        await axios
          .get(`https://jsonplaceholder.typicode.com/photos?albumId=${albumId}`)
          .then(response => {
            this.images = response.data;
            this.imagesList[albumId] = response.data;
          })
          .catch(error => {
            console.log(error);
          });

        this.loading = false;
      }
    }
  },

  components: {
    Images
  }
};
</script>