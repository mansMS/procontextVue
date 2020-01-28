<template>
  <section class="Posts">
    <div class="PostsBlock">
      <div class="AddPost">
        <div class="AddPost-ButtonBlock">
          <button
            @click="showForm = !showForm"
            :class="[showForm ? 'AddPost-Open' : 'AddPost-Close', 'AddPost-AddButton']"
          >{{showForm ? '&times;' : '+'}}</button>
        </div>

        <AddPostForm v-if="showForm" @newPostData="createPost($event)" />
      </div>
      <template v-if="loading">
        <div class="Spinner">
          <Spinner />
        </div>
      </template>
      <template v-else>
        <div>
          <ul class="PostsList">
            <li
              v-for="post in paginatedPosts"
              :key="post.id"
              :class="[post.id+'' === selectedPostId+'' && 'PostsList-SelectedPost', 'PostItem']"
            >
              <div
                @click="post.id+'' === selectedPostId+'' ? selectedPostId = '' : selectedPostId = post.id+''"
              >
                <p class="PostItem-CommentTitle">{{ post.title }}</p>
                <p>{{ post.body }}</p>
              </div>

              <div v-if="post.id+'' === selectedPostId+''">
                <Comments
                  :comments="comments"
                  :new-comment.sync="newComment"
                  @create-comment="createComment(post.id)"
                />
              </div>
            </li>
          </ul>
          <Pagination :pageCount="pageCount" :page-number.sync="pageNumber" />
        </div>
      </template>
    </div>
  </section>
</template>

<script>
import Spinner from "./Spinner";
import Comments from "./Comments.vue";
import Pagination from "./Pagination.vue";
import AddPostForm from "./AddPostForm.vue";
import axios from "axios";

const POST_PAGE_SIZE = 3;

export default {
  name: "Posts",
  components: {
    Comments,
    Pagination,
    AddPostForm,
    Spinner
  },
  data() {
    return {
      showForm: false,
      posts: [],
      selectedPostId: "",
      comments: [],
      pageNumber: 1,
      newComment: "",
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

    createPost: async function({ postTitle, postBody }) {
      this.showForm = false;
      const newPostObj = {
        id: this.posts.length + 1,
        userId: 12345,
        title: postTitle,
        body: postBody
      };
      axios
        .put(
          `https://jsonplaceholder.typicode.com/posts/${newPostObj.id}`,
          newPostObj
        )
        .then(response => console.log(response.status))
        .catch(error => console.log(error));
    },

    createComment: async function(postId) {
      console.log(this.newComment);
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

<style lang="scss" scoped>
.Posts {
  grid-area: Posts;
}

.PostsBlock {
  border: 1px solid #c2c2e5;
  border-right: none;
}

.AddPost {
  &-ButtonBlock {
    display: flex;
    flex-direction: column;
  }

  &-AddButton {
    background-color: #fff;
    border: none;
    border-bottom: 1px solid #d8d8ff;
    width: 30px;
    height: 30px;
    font-size: 20px;
    outline: none;
  }

  &-Open {
    border-left: 1px solid #d8d8ff;
    align-self: flex-end;
  }

  &-Close {
    border-right: 1px solid #d8d8ff;
  }
}

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

.PostsList {
  list-style-type: none;
  padding: 0;
  margin: 0;

  &-SelectedPost {
    background-color: #f9f9ff;
  }
}

.PostItem {
  padding: 10px;
  cursor: pointer;

  & + .PostItem {
    border-top: 1px solid #d8d8ff;
  }

  &-CommentTitle {
    font-weight: bold;
    font-size: 120%;
    margin-top: 10px;
  }
}
</style>
