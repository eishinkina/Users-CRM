<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- table   -->
        <table>
          <!-- head  -->
          <thead>
            <tr>
              <th @click="sort('name')">Name &#8595;</th>
              <th @click="sort('username')">Username &#8595;</th>
              <th @click="sort('city')">City &#8595;</th>
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
        <p style="text-align: center">
          <span>debug: sort: {{ currentSort }}, dir: {{ currentSortDir }}</span>
          <span>
            page: {{ this.page.current }}, lenght: {{ this.page.length }}
          </span>
        </p>
      </div>
    </section>
    <section>
      <div class="container">
        <div class="button-list">
          <div class="btn btnPrimary" @click="prevPage">&#8592;</div>
          <div class="btn btnPrimary" @click="nextPage">&#8594;</div>
        </div>
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
      page: {
        current: 1,
        length: 3,
      },
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
      return [...this.users]
        .sort((a, b) => {
          let aValue = a[this.currentSort];
          let bValue = b[this.currentSort];

          if (this.currentSort === "city") {
            aValue = a.address[this.currentSort];
            bValue = b.address[this.currentSort];
          }

          let mod = 1;
          if (this.currentSortDir === "desc") mod = -1;
          if (aValue < bValue) return -1 * mod;
          if (aValue > bValue) return 1 * mod;
          return 0;
        })
        .filter((row, index) => {
          let start = (this.page.current - 1) * this.page.length;
          let end = this.page.current * this.page.length;
          if (index >= start && index < end) {
            return true;
          }
        });
    },
  },
  methods: {
    sort(e) {
      if (e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === "asc" ? "desc" : "asc";
      }
      this.currentSort = e;
    },
    //pagination
    nextPage() {
      if (this.page.current * this.page.length < this.users.length) {
        this.page.current++;
      }
    },
    prevPage() {
      if (this.page.current > 1) {
        this.page.current--;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 16px;
}
.button-list {
  width: 100%;
  text-align: center;
  .btn {
    border-radius: 60px;
    margin: 0 20px;
  }
}
</style>
