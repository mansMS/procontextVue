<template>
  <section class="Photographers">
    <template v-if="loading">
      <div class="Spinner">
        <Spinner />
      </div>
    </template>
    <template v-else-if="users.length">
      <ul class="RecordList">
        <li
          v-for="user in users"
          :key="user.id"
          :class="[user.id === +userId && 'selectedRecord', 'RecordItem']"
          @click="$emit('update:user-id', user.id+'')"
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
import axios from "axios";

export default {
  name: "Photographers",
  components: {
    Spinner
  },
  props: {
    userId: String
  },
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

<style lang="scss">
.Photographers {
  grid-area: Photographers;
}

.RecordList {
  border: 1px solid #c2c2e5;
  list-style-type: none;
  padding: 0;
  margin: 0;
}
.RecordItem {
  padding: 10px 20px;
  cursor: pointer;

  & + .RecordItem {
    border-top: 1px solid #d8d8ff;
  }

  &:hover {
    background-color: #e6e6fa;
  }
}
.selectedRecord {
  background-color: #cfcfe1;
}
</style>