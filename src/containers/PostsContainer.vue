<template>
  <Posts
    :posts="posts.slice((pageNumber-1)*3, (pageNumber-1)*3+3)"
    :comments="comments"
    :posrForCommentsId="posrForCommentsId"
    :pageNumber="pageNumber"
    :pageCount="pageCount"
    v-bind:loading="loading"
    @show-comments="posrForCommentsId = $event"
    @go-to-page="pageNumber = $event"
    @create-post="newPost = $event"
    @create-comment="newCommentObj = $event"
  />
</template>

<script>
import Posts from "../components/Posts.vue";

export default {
  name: "PostsContainer",

  data() {
    return {
      posts: [],
      posrForCommentsId: "",
      comments: [],
      pageNumber: 1,
      pageCount: 1,
      newPost: {},
      newCommentObj: {},
      loading: false
    };
  },

  mounted() {
    this.fetchPosts();
  },

  watch: {
    posrForCommentsId(id) {
      id && this.fetchComments(id);
    },
    posts(posts) {
      this.pageCount = Math.ceil(posts.length / 3);
    },
    newPost(post) {
      this.createPost(post);
    },
    newCommentObj(commentObj) {
      this.createComment(commentObj);
    }
  },
  computed: {
    paginatedData() {
      const start = this.pageNumber * 3;
      const end = start + 3;
      return this.comments.slice(start, end);
    }
  },

  methods: {
    fetchPosts: async function() {
      this.loading = true;

      const axios = require("axios");

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
      const axios = require("axios");

      await axios
        .get(`https://jsonplaceholder.typicode.com/posts/${id}/comments`)
        .then(response => {
          this.comments = response.data;
        })
        .catch(error => {
          console.log(error);
        });
    },

    createPost: async function(post) {
      if (post.postBody.trim().length && post.postTitle.trim().length) {
        const axios = require("axios");
        const newPostObj = {
          id: this.posts.length + 1,
          userId: 12345,
          title: post.postTitle,
          body: post.postBody
        };
        axios
          .put(
            `https://jsonplaceholder.typicode.com/posts/${newPostObj.id}`,
            newPostObj
          )
          .then(response => console.log(response.status))
          .catch(error => console.log(error));
      }
    },

    createComment: async function(commentObj) {
      if (commentObj.body.trim().length) {
        const axios = require("axios");
        const newCommentObj = {
          id: this.comments.length + 1,
          email: "qwerty@mail.ru",
          name: "NameName",
          body: commentObj.body
        };

        axios
          .put(
            `https://jsonplaceholder.typicode.com/posts/${commentObj.postId}/comments`,
            newCommentObj
          )
          .then(response => console.log(response.status))
          .catch(error => console.log(error));
      }
    }
  },

  components: {
    Posts
  }
};
</script>