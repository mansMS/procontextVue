<template>
  <section class="Albums">
    <div v-if="loading" class="Spinner">
      <Spinner />
    </div>
    <ul v-else-if="albumsList[userId]" class="RecordList">
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
      return this.albumsList[this.userId].filter(function(album) {
        return album.title
          .toUpperCase()
          .includes(self.filterInput.toUpperCase());
      });
    }
  },
  methods: {
    fetchData: async function(userId) {
      if (!(userId in this.albumsList)) {
        this.loading = true;

        await axios
          .get(`https://jsonplaceholder.typicode.com/albums/?userId=${userId}`)
          .then(response => {
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