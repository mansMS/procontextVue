<template>
  <div>
    <ul class="CommentsList">
      <li
        v-for="comment in comments.slice(0, displayedCommentsCount)"
        :key="comment.id"
        class="CommentsList-Item"
      >
        <p>{{comment.email}}</p>
        <p>{{comment.body}}</p>
        <p>{{comment.name}}</p>
      </li>
      <div v-if="displayedCommentsCount < comments.length" class="AddCommentsBlock">
        <button class="AddCommentsBlock-AddButton" @click.stop="addComments">еще</button>
      </div>
    </ul>
    <div class="CreateComment">
      <input
        class="CreateComment-Area"
        placeholder="Введите комментарий"
        :value="newComment"
        @input="$emit('new-comment', $event)"
      />
      <button class="CreateComment-Button" @click.prevent="$emit('create-comment')">Отправить</button>
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

<style lang="scss">
.CommentsList {
  list-style-type: none;
  padding: 0;
  margin-bottom: 20px;
  font-size: 14px;
  padding: 0 10px;
  cursor: default;

  &-Item {
    border-top: 1px solid #484848;
  }
}
.AddCommentsBlock {
  text-align: center;

  &-AddButton {
    padding: 3px 8px;
    border: 1px solid #d8d8ff;
    background: #fff;
    cursor: pointer;
  }
}

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
