<template>
  <div class="Comments">
    <div v-if="loading" class="Spinner">
      <Spinner />
    </div>
    <div>
      <ul class="CommentsList">
        <li v-for="comment in filteComments" :key="comment.id" class="CommentsList-Item">
          <p>{{comment.email}}</p>
          <p>{{comment.body}}</p>
          <p>{{comment.name}}</p>
        </li>
        <div v-if="filteComments.length < comments.length" class="AddCommentsBlock">
          <button class="AddCommentsBlock-AddButton" @click.stop="addComments">еще</button>
        </div>
      </ul>
      <CreateComment @newComment="createComment($event)" />
    </div>
  </div>
</template>

<script>
import CreateComment from "./CreateComment.vue";
import Spinner from "./Spinner";
import axios from "axios";

const COMMENTS_PART_SIZE = 2;

export default {
  name: "Comments",
  components: {
    CreateComment,
    Spinner
  },
  props: {
    postId: String
  },
  data() {
    return {
      comments: [],
      filteComments: [],
      loading: false
    };
  },
  mounted() {
    this.fetchComments(this.postId);
  },
  methods: {
    addComments() {
      this.filteComments = this.comments.slice(
        0,
        this.filteComments.length + COMMENTS_PART_SIZE
      );
    },
    fetchComments: async function(id) {
      this.loading = true;
      await axios
        .get(`https://jsonplaceholder.typicode.com/posts/${id}/comments`)
        .then(response => {
          this.comments = response.data;
          this.filteComments = response.data.slice(0, COMMENTS_PART_SIZE);
        })
        .catch(error => {
          console.log(error);
        });
      this.loading = false;
    },
    createComment: async function(newComment) {
      if (newComment.trim().length) {
        const newCommentObj = {
          id: this.comments.length + 1,
          email: "qwerty@mail.ru",
          name: "NameName",
          body: this.newComment
        };
        axios
          .put(
            `https://jsonplaceholder.typicode.com/posts/${this.postId}/comments`,
            newCommentObj
          )
          .then(response => console.log(response.status))
          .catch(error => console.log(error));
      }
      this.newComment = "";
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

.Comments .SpinnerBlock {
  background: #f9f9ff;
}
</style>
