<template>
  <div class="App">
    <PhotographersContainer
      v-bind:userId="userId"
      v-on:select-user="userId = $event; albumId=''; imageId=''"
    />
    <AlbumsContainer
      v-bind:userId="userId"
      v-bind:albumId="albumId"
      v-on:select-album="albumId = $event; imageId=''"
    />
    <ImagesContainer
      v-bind:albumId="albumId"
      v-bind:imageId="imageId"
      v-on:select-image="imageId = $event"
    />
    <PostsContainer />
  </div>
</template>

<script>
import PhotographersContainer from "./containers/PhotographersContainer.vue";
import AlbumsContainer from "./containers/AlbumsContainer.vue";
import ImagesContainer from "./containers/ImagesContainer.vue";
import PostsContainer from "./containers/PostsContainer.vue";

export default {
  name: "app",
  data() {
    return {
      userId: "",
      albumId: "",
      imageId: ""
    };
  },
  components: {
    PhotographersContainer,
    AlbumsContainer,
    ImagesContainer,
    PostsContainer
  }
};
</script>

<style>
.App {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;

  /* width: 95%; */
  /* max-width: 1400px; */
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
</style>
