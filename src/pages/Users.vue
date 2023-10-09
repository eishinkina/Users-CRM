<template>
  <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <!-- table   -->
        <table>
          <!-- head  -->
          <thead>
            <tr>
              <th>Name</th>
              <th>Username</th>
              <th>City</th>
            </tr>
          </thead>
          <!-- body  -->
          <tbody>
            <tr v-for="user in users" :key="user.id">
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
