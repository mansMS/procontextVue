<template>
  <section class="Posts">
    <div>
      <button @click="showForm = !showForm">+</button>
      <form v-if="showForm">
        <div>
          <input placeholder="Введите название поста" v-model="postTitle" />
        </div>
        <div>
          <textarea placeholder="Введите текст поста" v-model="postBody" />
        </div>
        <button
          @click.prevent="$emit('create-post', {postTitle, postBody}); postTitle=''; postBody=''; showForm=false"
        >Добавить</button>
      </form>
    </div>
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else>
      <div>
        <ul class="PostsList">
          <li v-for="post in posts" :key="post.id" class="PostItem">
            <div
              @click="$emit('show-comments', post.id+'' === posrForCommentsId+'' ? '' : post.id+'')"
            >
              <p class="CommentTitle">{{ post.title }}</p>
              <p>{{ post.body }}</p>
            </div>

            <div v-if="post.id+'' === posrForCommentsId+''">
              <Comments
                :comments="comments"
                v-on:create-comment="$emit('create-comment', {body: $event, postId: post.id})"
              />
            </div>
          </li>
        </ul>
        <Pagination
          :pageNumber="pageNumber"
          :pageCount="pageCount"
          v-on:go-to-page="$emit('go-to-page', $event)"
        />
      </div>
    </template>
  </section>
</template>

<script>
import Spinner from "./Spinner";
import Comments from "./Comments.vue";
import Pagination from "./Pagination.vue";

export default {
  name: "Posts",
  components: {
    Comments,
    Pagination,
    Spinner
  },
  props: {
    posts: Array,
    comments: Array,
    posrForCommentsId: String,
    pageNumber: Number,
    pageCount: Number,
    loading: Boolean
  },
  data() {
    return {
      showForm: false,
      postTitle: "",
      postBody: ""
    };
  }
};
</script>

<style lang="scss" scoped>
.Posts {
  grid-area: Posts;
  border: 1px solid black;
  border-right: none;

  .PostsList {
    list-style-type: none;
    padding: 0;
    margin: 0;

    .PostItem {
      padding: 10px;
      border-bottom: 1px solid red;
      cursor: pointer;

      .CommentTitle {
        font-weight: bold;
        font-size: 120%;
        margin-top: 10px;
      }

      .CommentsList {
        list-style-type: none;
        padding: 0;
        margin: 0;
        font-size: 12px;
        padding: 0 10px;

        .CommentItem {
          border-top: 1px solid blue;
        }

        .AddComments {
          border: none;
          text-align: center;
        }
      }

      span {
        cursor: pointer;
      }
    }
  }

  .Spinner {
    text-align: center;
  }
}
</style>
