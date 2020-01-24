<template>
  <Images
    v-bind:images="images"
    v-bind:albumId="albumId"
    v-bind:imageId="imageId"
    v-on:select-image="$emit('select-image', $event)"
  />
</template>

<script>
import Images from "../components/Images.vue";

export default {
  name: "ImagesContainer",
  updated() {
    if (this.id !== this.albumId && this.albumId) {
      this.fetchData(this.albumId);
      this.id = this.albumId;
    }
  },
  props: ["albumId", "imageId"],
  data() {
    return {
      id: "",
      imagesList: {},
      images: []
    };
  },
  methods: {
    fetchData: async function(albumId) {
      let self = this;
      if (albumId in this.imagesList) {
        self.images = this.imagesList[albumId];
      } else {
        const getImages = new Request(
          `https://jsonplaceholder.typicode.com/photos?albumId=${albumId}`
        );

        fetch(getImages)
          .then(response => {
            return response.json();
          })
          .then(data => {
            self.images = data;
            self.imagesList[albumId] = data;
          })
          .catch(error => {
            console.log(error);
          });
      }
    }
  },
  components: {
    Images
  }
};
</script>