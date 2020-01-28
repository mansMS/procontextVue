<template>
  <form class="AddPostForm">
    <div>
      <input class="AddPostForm-Area" placeholder="Введите название поста" v-model="postTitle" />
    </div>
    <div>
      <textarea
        class="AddPostForm-Area"
        rows="5"
        placeholder="Введите текст поста"
        v-model="postBody"
      />
    </div>
    <button class="AddPostForm-Button" @click.prevent="createPost()">Добавить</button>
  </form>
</template>

<script>
export default {
  name: "AddPostForm",
  data() {
    return {
      postTitle: "",
      postBody: ""
    };
  },
  methods: {
    createPost: function() {
      if (this.postTitle.trim().length && this.postBody.trim().length) {
        this.$emit("newPostData", {
          postTitle: this.postTitle,
          postBody: this.postBody
        });
        this.postTitle = "";
        this.postBody = "";
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.AddPostForm {
  padding: 10px;

  &-Area {
    width: 100%;
    box-sizing: border-box;
    font-size: 14px;
    line-height: 14px;
    margin: 5px 0;
    padding: 4px 6px;
  }

  &-Button {
    padding: 3px 8px;
    border: 1px solid rgb(169, 169, 169);
    background: #fff;
    cursor: pointer;
  }
}
</style>
