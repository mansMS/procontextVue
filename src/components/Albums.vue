<template>
  <section class="Albums">
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else-if="albums.length">
      <ul class="AlbumsList">
        <li>
          <input v-model="filterInput" class="AlbumInput" placeholder="Введине название альбома" />
        </li>
        <li
          v-for="album in filterAlbums"
          :key="album.id"
          :class="[album.id+'' === albumId && 'selected', 'AlbumItem']"
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

<style lang="scss" scoped>
.Albums {
  grid-area: Albums;

  .AlbumsList {
    border: 1px solid #c2c2e5;
    list-style-type: none;
    padding: 0;
    margin: 0;

    .AlbumInput {
      box-sizing: border-box;
      width: 100%;
      padding: 10px 20px;
      font-size: 16px;
      border: none;
      border-bottom: 1px solid #d8d8ff;
    }

    .AlbumItem {
      padding: 10px 20px;
      cursor: pointer;

      &:hover {
        background-color: #e6e6fa;
      }

      & + .AlbumItem {
        border-top: 1px solid #d8d8ff;
      }
    }

    .selected {
      background-color: #cfcfe1;
    }
  }

  .Spinner {
    text-align: center;
  }
}
</style>