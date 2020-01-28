<template>
  <div class="CreateComment">
    <input class="CreateComment-Area" placeholder="Введите комментарий" v-model="newComment" />
    <button class="CreateComment-Button" @click.prevent="createComment()">Отправить</button>
  </div>
</template>

<script>
export default {
  name: "CreateComment",
  data() {
    return {
      newComment: ""
    };
  },
  methods: {
    createComment: function() {
      if (this.newComment.trim().length) {
        this.$emit("newComment", this.newComment);
        this.newComment = "";
      }
    }
  }
};
</script>

<style lang="scss">
.CreateComment {
  display: flex;

  &-Area {
    border: 1px solid #d8d8ff;
    flex-grow: 2;
    line-height: 20px;
    padding: 2px 7px;
    border-right: 0px solid #d8d8ff;
  }

  &-Button {
    border: 1px solid #d8d8ff;
    background: #fff;
  }
}
</style>
