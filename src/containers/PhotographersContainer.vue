<template>
  <Photographers
    v-bind:users="users"
    v-bind:userId="userId"
    v-bind:loading="loading"
    v-on:select-user="$emit('select-user', $event)"
  />
</template>

<script>
import Photographers from "../components/Photographers.vue";

export default {
  name: "PhotographersContainer",

  props: ["userId"],

  data() {
    return {
      users: [],
      loading: false
    };
  },

  mounted() {
    this.fetchData();
  },

  methods: {
    fetchData: async function() {
      this.loading = true;
      const getUsers = new Request(
        "https://jsonplaceholder.typicode.com/users"
      );

      await fetch(getUsers)
        .then(response => {
          return response.json();
        })
        .then(data => {
          this.users = data;
        })
        .catch(error => {
          console.log(error);
        });

      this.loading = false;
    }
  },

  components: {
    Photographers
  }
};
</script>