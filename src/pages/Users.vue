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
              <th>Age</th>
              <th>Gender</th>
            </tr>
          </thead>
          <!-- body  -->
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>{{ user.name }}</td>
              <td>{{ user.age }}</td>
              <td>{{ user.gender }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

<script>
// import axios from "axios";
export default {
  data() {
    return {
      users: [],
    };
  },
  created() {
    this.users = [
      {
        id: 1,
        name: "Jack",
        age: 22,
        gender: "male",
      },
      {
        id: 2,
        name: "Tom",
        age: 23,
        gender: "male",
      },
    ];
  },
};
</script>

<style lang="scss" scoped></style>
