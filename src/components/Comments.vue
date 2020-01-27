<template>
  <div>
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
        <button @click.stop="displayedCommentsCount += 3">+</button>
      </li>
      <li>
        <input placeholder="Введите комментарий" v-model="newComment" />
        <button @click.prevent="$emit('create-comment', newComment); newComment=''">Отправить</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Comments",
  props: {
    comments: Array
  },
  data() {
    return {
      displayedCommentsCount: 3,
      newComment: ""
    };
  }
  //   methods: {
  //     addComments() {
  //       this.displayedCommentsCount += 10;
  //     }
  //   },
  //   computed: {
  //     filterComments() {
  //       console.log(this.displayedCommentsCount);
  //       return this.comments.slice(0, this.displayedCommentsCount);
  //     }
  //   }
};
</script>

<style lang="scss" scoped>
.CommentsList {
  list-style-type: none;
  padding: 0;
  margin: 0;
  font-size: 12px;
  padding: 0 10px;
  cursor: default;

  .CommentItem {
    border-top: 1px solid blue;
  }

  .AddComments {
    border: none;
    text-align: center;
  }
}
</style>
