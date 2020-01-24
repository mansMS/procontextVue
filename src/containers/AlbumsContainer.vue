<template>
  <Albums
    v-bind:albums="albums"
    v-bind:userId="userId"
    v-bind:albumId="albumId"
    v-on:select-album="$emit('select-album', $event)"
  />
</template>

<script>
import Albums from "../components/Albums.vue";

export default {
  name: "AlbumsContainer",
  updated() {
    if (this.id !== this.userId && this.userId) {
      this.fetchData(this.userId);
      this.id = this.userId;
    }
  },
  props: ["userId", "albumId"],
  data() {
    return {
      id: "",
      albumsList: {},
      albums: []
    };
  },
  methods: {
    fetchData: async function(userId) {
      let self = this;
      if (userId in this.albumsList) {
        self.albums = this.albumsList[userId];
      } else {
        const getAlbums = new Request(
          `https://jsonplaceholder.typicode.com/albums/?userId=${userId}`
        );

        fetch(getAlbums)
          .then(response => {
            return response.json();
          })
          .then(data => {
            self.albums = data;
            self.albumsList[userId] = data;
          })
          .catch(error => {
            console.log(error);
          });
      }
    }
  },
  components: {
    Albums
  }
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
</style>