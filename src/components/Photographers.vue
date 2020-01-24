<template>
  <section class="Photographers">
    <template v-if="users.length">
      <ul class="PhotographersList">
        <li
          v-for="user in users"
          v-bind:key="user.id"
          v-bind:class="[user.id === +userId && 'selected', 'PhotographerItem']"
          v-on:click="$emit('select-user', user.id+'')"
        >{{ user.name }}</li>
      </ul>
    </template>
    <template v-else>
      <p>Нет данных</p>
    </template>
  </section>
</template>



<script>
export default {
  name: "Photographers",
  props: {
    users: Array,
    userId: String
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
}
</style>