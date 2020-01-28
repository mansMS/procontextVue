<template>
  <section class="Photographers">
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else-if="users.length">
      <ul class="PhotographersList">
        <li
          v-for="user in users"
          :key="user.id"
          :class="[user.id === +userId && 'selected', 'PhotographerItem']"
          @click="$emit('select-user', user.id+'')"
        >{{ user.name }}</li>
      </ul>
    </template>
    <template v-else>
      <p>Нет данных</p>
    </template>
  </section>
</template>



<script>
import Spinner from "./Spinner";

export default {
  name: "Photographers",
  props: {
    users: Array,
    userId: String,
    loading: Boolean
  },

  components: {
    Spinner
  }
};
</script>

<style lang="scss" scoped>
.Photographers {
  grid-area: Photographers;

  .PhotographersList {
    border: 1px solid #c2c2e5;
    list-style-type: none;
    padding: 0;
    margin: 0;

    .PhotographerItem {
      padding: 10px 20px;
      cursor: pointer;

      &:hover {
        background-color: #e6e6fa;
      }

      & + .PhotographerItem {
        border-top: 1px solid #d8d8ff;
      }
    }

    .selected {
      background-color: #cfcfe1;
    }
  }

  .Spinner {
    text-align: center;
  }
}
</style>