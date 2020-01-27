<template>
  <Albums
    v-bind:albums="albums"
    v-bind:userId="userId"
    v-bind:albumId="albumId"
    v-bind:loading="loading"
    v-on:select-album="$emit('select-album', $event)"
  />
</template>

<script>
import Albums from "../components/Albums.vue";

export default {
  name: "AlbumsContainer",

  props: ["userId", "albumId"],

  data() {
    return {
      albumsList: {},
      albums: [],
      loading: false
    };
  },

  watch: {
    userId(id) {
      this.fetchData(id);
    }
  },

  methods: {
    fetchData: async function(userId) {
      if (userId in this.albumsList) {
        this.albums = this.albumsList[userId];
      } else {
        this.loading = true;

        const axios = require("axios");

        await axios
          .get(`https://jsonplaceholder.typicode.com/albums/?userId=${userId}`)
          .then(response => {
            this.albums = response.data;
            this.albumsList[userId] = response.data;
          })
          .catch(error => {
            console.log(error);
          });

        this.loading = false;
      }
    }
  },

  components: {
    Albums
  }
};
</script>