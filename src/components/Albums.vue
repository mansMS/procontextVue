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
          @click="$emit('select-album', album.id+'')"
        >{{ album.title }}</li>
      </ul>
    </template>
  </section>
</template>

<script>
import Spinner from "./Spinner";

export default {
  name: "Albums",
  components: {
    Spinner
  },
  props: {
    albums: Array,
    albumId: String,
    loading: Boolean
  },
  data() {
    return {
      filterInput: ""
    };
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