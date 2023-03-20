<script>
  import axios from "axios";
  export default {
    data() {
      return {
        items: [],
        user: {},
        email: '',
        name: '',
      };
    },
    methods: {
    async getItems() {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/users')
        this.items = response.data
      } catch (error) {
        console.error(error)
      }
    },

    async storeUser() {
      try {
        const user = await axios.post(
          "https://jsonplaceholder.typicode.com/users",
          {
            name: this.name,
            email: this.email
          }
        );

        console.log(user)
      } catch (e) {
        console.log(e);
      }
    },
    async updateUser() {
      try {
        const user = await axios.put(
          "https://jsonplaceholder.typicode.com/users/" + this.user.id,
          {
            name: this.user.name,
            email: this.user.email,
          }
        );

        console.log(user.data);
        alert("User updated!");
      } catch (e) {
        console.log(e);
      }
    },
    async editUser(user) {
      this.user.name = user.name;
      this.user.email = user.email;
      this.user.id = user.id;
    },
    async deleteUser(id) {
      let x = window.confirm("You want to delete the user?");

      if (x) {
        const user = await axios.delete(
          "https://jsonplaceholder.typicode.com/users/" + id
        );

        console.log(user);
        alert("User deleted!");
      }
    },
  },
  };
</script>

<template>
  <!-- READ USER-->
  <div>
      <button @click="getItems">Get Items</button>
      <ol>
        <li v-for="item in items" :key="item.id">{{ item.name }} : {{ item.email }}</li>
      </ol>
  </div>

  <!-- CREATE USER-->
  <div class="user-add">
    <h2>Add user</h2>
    <input type="text" v-model="name" /> <br />
    <input type="text" v-model="email" /> <br />

    <button @click="storeUser">Store</button>
  </div>

  <!-- UPDATE -->
  <div class="user-update">
    <h2>Update user</h2>
    <input type="text" v-model="user.name" /> <br />
    <input type="text" v-model="user.email" /> <br />

    <button @click="updateUser">Update</button>
    <button @click="editUser(user)">Edit</button>
  </div>

  <!-- DELETE -->
  <div class="users" v-for="item in items" :key="item.id">
    <h2>{{ item.name }}</h2>
    <p>{{ item.email }}</p>

    <button @click="editUser(user)">Edit</button>
    <button @click="deleteUser(user.id)">Delete</button>
  </div>
</template>

<style scoped>
</style>
