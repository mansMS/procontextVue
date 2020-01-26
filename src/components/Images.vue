<template>
  <section class="Images">
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else-if="imageId">
      <SelectedImage v-bind:imageURL="images.filter(image => image.id+'' === imageId)[0].url" />
    </template>
    <div class="ImagesList">
      <div v-for="image in filterImages" v-bind:key="image.id" class="ItemBlock">
        <div v-bind:class="[image.id+'' === imageId && 'selected']">
          <img
            v-bind:src="image.thumbnailUrl"
            alt="thumbnailImage"
            class="ImageItem"
            v-on:click="$emit('select-image', image.id+'')"
          />
        </div>
      </div>
    </div>
    <template v-if="displayedImagesCount < images.length">
      <div class="AddImage">
        <button v-on:click="addImages">Еще</button>
      </div>
    </template>
  </section>
</template>

<script>
import SelectedImage from "./SelectedImage.vue";
import Spinner from "./Spinner";

export default {
  name: "Images",
  components: {
    SelectedImage,
    Spinner
  },
  props: {
    images: Array,
    imageId: String,
    loading: Boolean
  },
  data() {
    return {
      displayedImagesCount: 10
    };
  },
  methods: {
    addImages() {
      this.displayedImagesCount += 10;
    }
  },
  computed: {
    filterImages() {
      return this.images.slice(0, this.displayedImagesCount);
    }
  }
};
</script>

<style lang="scss" scoped>
.Images {
  grid-area: Images;

  .ImagesList {
    display: flex;
    flex-wrap: wrap;

    .ItemBlock {
      text-align: center;
      flex-grow: 1;

      .ImageItem {
        margin-bottom: 15px;
        cursor: pointer;

        &:hover {
          opacity: 0.6;
        }
      }
    }

    .selected {
      position: relative;
      display: inline-block;

      &:after {
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 19px;
        background: rgba(0, 0, 0, 0.6);
        opacity: 1;
        cursor: pointer;
      }
    }
  }

  .AddImage {
    text-align: center;

    button {
      width: 80px;
      height: 40px;
      border: 1px solid #c2c2e5;
      background: none;
    }
  }

  .Spinner {
    text-align: center;
  }
}
</style>
