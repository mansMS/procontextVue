<template>
  <section class="Posts">
    <div class="PostsBlock">
      <div class="AddPost">
        <div class="AddButtonBlock">
          <button
            @click="showForm = !showForm"
            :class="[showForm ? 'open' : 'close']"
          >{{showForm ? '&times;' : '+'}}</button>
        </div>

        <form v-if="showForm">
          <div>
            <input
              placeholder="Введите название поста"
              :value="postTitle"
              @input="$emit('update:post-title', $event.target.value)"
            />
          </div>
          <div>
            <textarea
              rows="5"
              placeholder="Введите текст поста"
              :value="postBody"
              @input="$emit('update:post-body', $event.target.value)"
            />
          </div>
          <button @click.prevent="$emit('create-post'); showForm=false">Добавить</button>
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
            <li
              v-for="post in posts"
              :key="post.id"
              :class="[post.id+'' === selectedPostId+'' && 'selected', 'PostItem']"
            >
              <div
                @click="$emit('update:selected-post-id', post.id+'' === selectedPostId+'' ? '' : post.id+'')"
              >
                <p class="CommentTitle">{{ post.title }}</p>
                <p>{{ post.body }}</p>
              </div>

              <div v-if="post.id+'' === selectedPostId+''">
                <Comments
                  :comments="comments"
                  :newComment="newComment"
                  @new-comment="$emit('update:new-comment', $event.target.value)"
                  @create-comment="$emit('create-comment', post.id)"
                />
              </div>
            </li>
          </ul>
          <Pagination
            :pageNumber="pageNumber"
            :pageCount="pageCount"
            @go-to-page="$emit('update:go-to-page', $event)"
          />
        </div>
      </template>
    </div>
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
    selectedPostId: String,
    pageNumber: Number,
    pageCount: Number,
    postTitle: String,
    postBody: String,
    newComment: String,
    loading: Boolean
  },
  data() {
    return {
      showForm: false
    };
  }
};
</script>

<style lang="scss" scoped>
.Posts {
  grid-area: Posts;

  .PostsBlock {
    border: 1px solid #c2c2e5;
    border-right: none;

    .AddPost {
      .AddButtonBlock {
        display: flex;
        flex-direction: column;

        button {
          background-color: #fff;
          border: none;
          border-bottom: 1px solid #d8d8ff;
          width: 30px;
          height: 30px;
          font-size: 20px;
          outline: none;
        }

        .open {
          border-left: 1px solid #d8d8ff;
          align-self: flex-end;
        }

        .close {
          border-right: 1px solid #d8d8ff;
        }
      }

      form {
        padding: 10px;

        div {
          input,
          textarea {
            width: 100%;
            box-sizing: border-box;
            font-size: 14px;
            line-height: 14px;
            margin: 5px 0;
            padding: 4px 6px;
          }
        }

        button {
          padding: 3px 8px;
          border: 1px solid rgb(169, 169, 169);
          background: #fff;
          cursor: pointer;
        }
      }
    }

    .PostsList {
      list-style-type: none;
      padding: 0;
      margin: 0;

      .PostItem {
        padding: 10px;
        cursor: pointer;

        .CommentTitle {
          font-weight: bold;
          font-size: 120%;
          margin-top: 10px;
        }

        span {
          cursor: pointer;
        }

        & + .PostItem {
          border-top: 1px solid #d8d8ff;
        }
      }
    }

    .selected {
      background-color: #f9f9ff;
    }

    .Spinner {
      text-align: center;
    }
  }
}
</style>
