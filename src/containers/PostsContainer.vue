<template>
  <Posts
    :comments="comments"
    :posts="paginatedPosts"
    :pageCount="pageCount"
    :selected-post-id.sync="selectedPostId"
    :post-title.sync="postTitle"
    :post-body.sync="postBody"
    :new-comment.sync="newComment"
    :page-number.sync="pageNumber"
    @create-post="createPost()"
    @create-comment="createComment($event)"
    :loading="loading"
  />
</template>

<script>
import Posts from "../components/Posts.vue";
import axios from "axios";

const POST_PAGE_SIZE = 3;

export default {
  name: "PostsContainer",

  components: {
    Posts
  },

  data() {
    return {
      posts: [],
      selectedPostId: "",
      comments: [],
      pageNumber: 1,
      postTitle: "",
      postBody: "",
      newComment: "",
      newCommentPostId: "",
      loading: false
    };
  },

  mounted() {
    this.fetchPosts();
  },

  watch: {
    selectedPostId(id) {
      id && this.fetchComments(id);
      this.newComment = "";
    }
  },
  computed: {
    pageCount: function() {
      return Math.ceil(this.posts.length / POST_PAGE_SIZE);
    },
    paginatedPosts: function() {
      return this.posts.slice(
        (this.pageNumber - 1) * POST_PAGE_SIZE,
        (this.pageNumber - 1) * POST_PAGE_SIZE + POST_PAGE_SIZE
      );
    }
  },

  methods: {
    fetchPosts: async function() {
      this.loading = true;

      await axios
        .get("https://jsonplaceholder.typicode.com/posts")
        .then(response => {
          this.posts = response.data;
        })
        .catch(error => {
          console.log(error);
        });

      this.loading = false;
    },

    fetchComments: async function(id) {
      await axios
        .get(`https://jsonplaceholder.typicode.com/posts/${id}/comments`)
        .then(response => {
          this.comments = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },

    createPost: async function() {
      if (this.postBody.trim().length && this.postTitle.trim().length) {
        const newPostObj = {
          id: this.posts.length + 1,
          userId: 12345,
          title: this.postTitle,
          body: this.postBody
        };
        axios
          .put(
            `https://jsonplaceholder.typicode.com/posts/${newPostObj.id}`,
            newPostObj
          )
          .then(response => console.log(response.status))
          .catch(error => console.log(error));
      }
      this.postTitle = "";
      this.postBody = "";
    },

    createComment: async function(postId) {
      if (this.newComment.trim().length) {
        const newCommentObj = {
          id: this.comments.length + 1,
          email: "qwerty@mail.ru",
          name: "NameName",
          body: this.newComment
        };
        axios
          .put(
            `https://jsonplaceholder.typicode.com/posts/${postId}/comments`,
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