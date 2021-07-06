<template>
  <div>
    <br />
    <button @click="createUser">Create</button>
    <label for="NIK">NIK</label>
    <input
      v-model="userCreate.NIK"
      type="text"
      id="NIK"
      name="NIK"
    /><br /><br />
    <label for="Name">Name</label>
    <input
      v-model="userCreate.Name"
      type="text"
      id="Name"
      name="Name"
    /><br /><br />
    <label for="Company">Company</label>
    <input
      v-model="userCreate.Company"
      type="text"
      id="Company"
      name="Company"
    /><br /><br />
    <h1>List Users :</h1>
    <ul>
      <li v-for="(user, index) in users" :key="index">
        <input v-model="user.NIK" type="text" />
        <input v-model="user.Name" type="text" />
        <input v-model="user.Company" type="text" />
        <button @click="updateUser(user)">Update</button>
        <button @click="deleteUser(index, user.NIK)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data: () => ({
    users: [],
    user: {
      NIK: '',
      Name: '',
      Company: '',
    },
    userCreate: {
      NIK: '',
      Name: '',
      Company: '',
    },
  }),
  created() {
    axios
      .get('https://golang-service-ludesdeveloper.cloud.okteto.net/users')
      .then((response) => (this.users = response.data))
    // .then((response) => console.log(response))
  },
  methods: {
    async createUser() {
      await axios.post(
        'https://golang-service-ludesdeveloper.cloud.okteto.net/users',
        this.userCreate
      )
      await this.users.push(this.userCreate)
      this.userCreate = {
        NIK: '',
        Name: '',
        Company: '',
      }
    },
    updateUser(user) {
      axios.put(
        'https://golang-service-ludesdeveloper.cloud.okteto.net/users',
        user
      )
      // this.users.push(this.user)
    },
    deleteUser(index, nik) {
      axios.delete(
        'https://golang-service-ludesdeveloper.cloud.okteto.net/users',
        {
          data: {
            NIK: nik,
          },
        }
      )
      this.users.splice(index)
    },
  },
}
</script>

<style></style>
