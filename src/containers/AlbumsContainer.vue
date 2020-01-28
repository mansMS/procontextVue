<template>
  <Albums
    :albums="albums"
    :userId="userId"
    :albumId="albumId"
    :loading="loading"
    @select-album="$emit('update:select-album', $event)"
  />
</template>

<script>
import Albums from "../components/Albums.vue";
import axios from "axios";

export default {
  name: "AlbumsContainer",

  components: {
    Albums
  },

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
  }
};
</script>