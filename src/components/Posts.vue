<template>
  <section class="Posts">
    <div class="PostsBlock">
      <div class="AddPost">
        <div class="AddPost-ButtonBlock">
          <button
            @click="showAddForm = !showAddForm"
            :class="[showAddForm ? 'AddPost-Open' : 'AddPost-Close', 'AddPost-AddButton']"
          >{{showAddForm ? '&times;' : '+'}}</button>
        </div>

        <AddPostForm v-if="showAddForm" @newPostData="createPost($event)" />
      </div>
      <div v-if="loading" class="Spinner">
        <Spinner />
      </div>
      <div v-else>
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
              <Comments :postId="selectedPostId" />
            </div>
          </li>
        </ul>
        <Pagination :pageCount="pageCount" :page-number.sync="pageNumber" />
      </div>
    </div>
  </section>
</template>

<script>
import Comments from "./Comments.vue";
import Pagination from "./Pagination.vue";
import AddPostForm from "./AddPostForm.vue";
import Spinner from "./Spinner";
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
      showAddForm: false,
      posts: [],
      selectedPostId: "",
      pageNumber: 1,
      loading: false
    };
  },
  mounted() {
    this.fetchPosts();
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

    createPost: async function({ postTitle, postBody }) {
      this.showAddForm = false;
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
