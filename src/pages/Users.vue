<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- table   -->
        <table>
          <!-- head  -->
          <thead>
            <tr>
              <th @click="sort('name')">Name</th>
              <th @click="sort('username')">Username</th>
              <th @click="sort('city')">City</th>
            </tr>
          </thead>
          <!-- body  -->
          <tbody>
            <tr v-for="user in sortedUsers" :key="user.id">
              <td>
                <img
                  src="https://via.placeholder.com/150/92c952"
                  :alt="user.name"
                />
                <span>
                  {{ user.name }}
                </span>
              </td>
              <td>{{ user.username }}</td>
              <td>{{ user.address.city }}</td>
            </tr>
          </tbody>
        </table>
        <p>debug: sort: {{ currentSort }}, dir: {{ currentSortDir }}</p>
      </div>
    </section>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      users: [],
      currentSort: "name",
      currentSortDir: "asc",
    };
  },
  created() {
    axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then((response) => {
        this.users = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  computed: {
    sortedUsers() {
      return [...this.users].sort((a, b) => {
        let aValue = a[this.currentSort];
        let bValue = b[this.currentSort];

        if (this.currentSort === 'city') {
          aValue = a.address[this.currentSort];
          bValue = b.address[this.currentSort];
        }

        let mod = 1;
        if (this.currentSortDir === 'desc') mod = -1;
        if (aValue < bValue) return -1 * mod;
        if (aValue > bValue) return 1 * mod;
        return 0;
      });
    }
  },
  methods: {
    sort(e) {
      if (e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      }
      this.currentSort = e;
    }
  }
};
</script>

<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 16px;
}
</style>
