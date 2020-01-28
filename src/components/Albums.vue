<template>
  <section class="Albums">
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else-if="albums.length">
      <ul class="RecordList">
        <li>
          <input
            v-model="filterInput"
            class="Albums-FilterInput"
            placeholder="Введине название альбома"
          />
        </li>
        <li
          v-for="album in filterAlbums"
          :key="album.id"
          :class="[album.id+'' === albumId && 'selectedRecord', 'RecordItem']"
          @click="$emit('update:album-id', album.id+'')"
        >{{ album.title }}</li>
      </ul>
    </template>
  </section>
</template>

<script>
import Spinner from "./Spinner";
import axios from "axios";

export default {
  name: "Albums",
  components: {
    Spinner
  },
  props: {
    userId: String,
    albumId: String
  },
  data() {
    return {
      albumsList: {},
      albums: [],
      filterInput: "",
      loading: false
    };
  },
  watch: {
    userId(id) {
      this.fetchData(id);
    }
  },
  computed: {
    filterAlbums() {
      let self = this;
      return this.albums.filter(function(album) {
        return album.title
          .toUpperCase()
          .includes(self.filterInput.toUpperCase());
      });
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

<style lang="scss" >
.Albums {
  grid-area: Albums;

  &-FilterInput {
    box-sizing: border-box;
    width: 100%;
    padding: 10px 20px;
    font-size: 16px;
    border: none;
    border-bottom: 1px solid #d8d8ff;
  }
}
</style>