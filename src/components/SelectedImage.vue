<template>
  <section class="SelectedImage">
    <img v-bind:src="imageURL" />
  </section>
</template>

<script>
export default {
  name: "SelectedImage",
  props: {
    imageURL: String
  }
};
</script>

<style lang="scss" scoped>
.SelectedImage {
  text-align: center;
  margin-bottom: 1vw;
}
</style>
