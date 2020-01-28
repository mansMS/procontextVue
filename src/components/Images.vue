<template>
  <section class="Images">
    <div v-if="loading" class="Spinner">
      <Spinner />
    </div>
    <div v-else-if="imagesList[albumId]">
      <div v-if="imageId" class="Images-SelectedImage">
        <img :src="imageURL" />
      </div>
      <div class="ImagesList">
        <div v-for="image in filterImages" :key="image.id" class="ImagesList-ImageBlock">
          <div :class="[image.id+'' === imageId && 'ImagesList-SelectedImg']">
            <img
              :src="image.thumbnailUrl"
              alt="thumbnailImage"
              class="ImagesList-Item"
              @click="$emit('update:image-id', image.id+'')"
            />
          </div>
        </div>
      </div>
      <div v-if="this.filterImages.length < imagesList[albumId].length" class="AddButtonBlock">
        <button class="Images-AddImageBtn" @click="addImages">Еще</button>
      </div>
    </div>
  </section>
</template>

<script>
import Spinner from "./Spinner";
import axios from "axios";

const IMAGES_PART_SIZE = 10;

export default {
  name: "Images",
  components: {
    Spinner
  },
  props: {
    albumId: String,
    imageId: String
  },
  data() {
    return {
      imagesList: {},
      filterImages: [],
      loading: false
    };
  },
  watch: {
    albumId(id) {
      this.fetchData(id);
    }
  },
  computed: {
    imageURL() {
      return this.imagesList[this.albumId].filter(
        image => image.id + "" === this.imageId
      )[0].url;
    }
  },
  methods: {
    addImages() {
      this.filterImages = this.imagesList[this.albumId].slice(
        0,
        this.filterImages.length + IMAGES_PART_SIZE
      );
    },

    fetchData: async function(albumId) {
      if (!(albumId in this.imagesList)) {
        this.loading = true;

        await axios
          .get(`https://jsonplaceholder.typicode.com/photos?albumId=${albumId}`)
          .then(response => {
            this.imagesList[albumId] = response.data;
            this.filterImages = response.data.slice(0, IMAGES_PART_SIZE);
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
