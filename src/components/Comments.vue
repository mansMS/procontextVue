<template>
  <div class="CommentsBlock">
    <ul class="CommentsList">
      <li
        v-for="comment in comments.slice(0, displayedCommentsCount)"
        :key="comment.id"
        class="CommentItem"
      >
        <p>{{comment.email}}</p>
        <p>{{comment.body}}</p>
        <p>{{comment.name}}</p>
      </li>
      <li v-if="displayedCommentsCount < comments.length" class="AddComments">
        <button @click.stop="addComments">еще</button>
      </li>
    </ul>
    <div class="CreateComment">
      <input
        placeholder="Введите комментарий"
        :value="newComment"
        @input="$emit('new-comment', $event)"
      />
      <button @click.prevent="$emit('create-comment')">Отправить</button>
    </div>
  </div>
</template>

<script>
const COMMENTS_PAGE_SIZE = 2;

export default {
  name: "Comments",
  props: {
    comments: Array,
    newComment: String
  },
  data() {
    return {
      displayedCommentsCount: COMMENTS_PAGE_SIZE
    };
  },
  methods: {
    addComments() {
      this.displayedCommentsCount += COMMENTS_PAGE_SIZE;
    }
  }
};
</script>

<style lang="scss" scoped>
.CommentsList {
  list-style-type: none;
  padding: 0;
  margin-bottom: 20px;
  font-size: 14px;
  padding: 0 10px;
  cursor: default;

  .CommentItem {
    border-top: 1px solid #484848;
  }

  .AddComments {
    border: none;
    text-align: center;

    button {
      padding: 3px 8px;
      border: 1px solid #d8d8ff;
      background: #fff;
      cursor: pointer;
    }
  }
}

.CreateComment {
  display: flex;
  input {
    border: 1px solid #d8d8ff;
    flex-grow: 2;
    line-height: 20px;
    padding: 2px 7px;
    border-right: 0px solid #d8d8ff;
  }

  button {
    border: 1px solid #d8d8ff;
    background: #fff;
  }
}
</style>
