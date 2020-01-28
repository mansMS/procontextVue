<template>
  <section class="Images">
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else-if="imageId">
      <div class="Images-SelectedImage">
        <img :src="imageURL" />
      </div>
    </template>
    <div class="ImagesList">
      <div v-for="image in filterImages" :key="image.id" class="ImagesList-ImageBlock">
        <div :class="[image.id+'' === imageId && 'ImagesList-SelectedImg']">
          <img
            :src="image.thumbnailUrl"
            alt="thumbnailImage"
            class="ImagesList-Item"
            @click="$emit('select-image', image.id+'')"
          />
        </div>
      </div>
    </div>
    <template v-if="displayedImagesCount < images.length">
      <div class="AddButtonBlock">
        <button class="Images-AddImageBtn" @click="addImages">Еще</button>
      </div>
    </template>
  </section>
</template>

<script>
import Spinner from "./Spinner";

export default {
  name: "Images",
  components: {
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
  computed: {
    filterImages() {
      return this.images.slice(0, this.displayedImagesCount);
    },
    imageURL() {
      return this.images.filter(image => image.id + "" === this.imageId)[0].url;
    }
  },
  methods: {
    addImages() {
      this.displayedImagesCount += 10;
    }
  }
};
</script>

<style lang="scss" scoped>
.Images {
  grid-area: Images;

  &-SelectedImage {
    text-align: center;
    margin-bottom: 1vw;
  }

  &-AddImageBtn {
    width: 80px;
    height: 40px;
    border: 1px solid #c2c2e5;
    background: none;
  }
}

.ImagesList {
  display: flex;
  flex-wrap: wrap;

  &-ImageBlock {
    text-align: center;
    flex-grow: 1;
  }

  &-Item {
    margin-bottom: 15px;
    cursor: pointer;

    &:hover {
      opacity: 0.6;
    }
  }

  &-SelectedImg {
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

.AddButtonBlock {
  text-align: center;
}
</style>
