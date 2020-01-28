<template>
  <Images
    :images="images"
    :albumId="albumId"
    :imageId="imageId"
    :loading="loading"
    @select-image="$emit('update:select-image', $event)"
  />
</template>

<script>
import Images from "../components/Images.vue";
import axios from "axios";

export default {
  name: "ImagesContainer",

  components: {
    Images
  },

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
  }
};
</script>