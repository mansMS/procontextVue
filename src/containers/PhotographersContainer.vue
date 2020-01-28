<template>
  <Photographers
    :users="users"
    :userId="userId"
    :loading="loading"
    @select-user="$emit('update:select-user', $event)"
  />
</template>

<script>
import Photographers from "../components/Photographers.vue";
import axios from "axios";

export default {
  name: "PhotographersContainer",

  components: {
    Photographers
  },

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

      await axios
        .get("https://jsonplaceholder.typicode.com/users")
        .then(response => (this.users = response.data))
        .catch(error => {
          console.log(error);
        })
        .catch(error => {
          console.log(error);
        });

      this.loading = false;
    }
  }
};
</script>