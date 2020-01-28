<template>
  <div class="App">
    <Photographers :user-id.sync="userId" />
    <Albums :userId="userId" :album-id.sync="albumId" />
    <Images :albumId="albumId" :image-id.sync="imageId" />
    <Posts />
  </div>
</template>

<script>
import Photographers from "./components/Photographers.vue";
import Albums from "./components/Albums.vue";
import Images from "./components/Images.vue";
import Posts from "./components/Posts.vue";

export default {
  name: "app",

  components: {
    Photographers,
    Albums,
    Images,
    Posts
  },

  data() {
    return {
      userId: "",
      albumId: "",
      imageId: ""
    };
  },

  watch: {
    userId() {
      this.albumId = "";
      this.imageId = "";
    },
    albumId() {
      this.imageId = "";
    }
  }
};
</script>

<style>
.App {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin: 30px auto;
  display: grid;
  grid-template-columns: 3fr 5fr 3fr;
  grid-template-rows: auto 1fr;
  grid-template-areas:
    "Photographers Albums Posts"
    "Images Images Posts";
  grid-gap: 2vw;
}

@media (max-width: 600px) {
  .App {
    grid-template-columns: 1fr;
    grid-template-areas: "Posts Photographers" "Albums" "Images";
  }
}

body {
  margin: 0;
  margin-left: 20px;
}

.Spinner {
  text-align: center;
}
</style>
