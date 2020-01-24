<template>
  <Photographers
    v-bind:users="users"
    v-bind:userId="userId"
    v-on:select-user="$emit('select-user', $event)"
  />
</template>

<script>
import Photographers from "../components/Photographers.vue";

export default {
  name: "PhotographersContainer",
  mounted() {
    this.fetchData();
  },
  props: ["userId"],
  data() {
    return {
      users: []
    };
  },
  methods: {
    fetchData: async function() {
      let self = this;
      const getUsers = new Request(
        "https://jsonplaceholder.typicode.com/users"
      );

      fetch(getUsers)
        .then(response => {
          return response.json();
        })
        .then(data => {
          self.users = data;
          // console.log(self.users);
        })
        .catch(error => {
          console.log(error);
        });
    }
  },
  components: {
    Photographers
  }
};
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
</style>