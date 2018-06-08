<template lang="pug">
  .app
    table
      tr
        th
        th id
        th name
        th surname
        th date of birth
        th mobile number
        th address
      tr(v-for="user in users" :key="user.id")
        td {{user.id}}
        td {{user.name}}
        td {{user.surname}}
        td {{user.dateOfBirth}}
        td {{user.mobile}}
        td {{user.address}}
        button.edit(@click="editUser(user.id)") edit
        button.delete(@click="deleteUser(user.id)") delete

    form
      input.id(type="text" :placeholder="form.name" v-model="form.name")
      input.id(type="text" :placeholder="form.surname" v-model="form.surname")
      input.id(type="text" :placeholder="form.dateOfBirth" v-model="form.dateOfBirth")
      input.id(type="text" :placeholder="form.mobile" v-model="form.mobile")
      input.id(type="text" :placeholder="form.address" v-model="form.address")
      button.edit(@click.prevent="saveUser(form.id)") save
</template>

<script>
import axios from 'axios'

export default {
  name: 'app',
  data() {
    return {
      users: [],
      form: {
        id: "id",
        name: "name",
        surname: "surname",
        dateOfBirth: "date of birth",
        mobile: "mobile number",
        address: "address"
      },
      showForm: false,
    }
  },
  methods: {
    getUsers() {
      axios.get('http://localhost:3000/users')
      .then(response => { 
        this.users = response.data 
      })
      .catch(errors => { 
        console.log(errors) 
      })
    },
    editUser(userId) {
      let editedUser = this.users.filter(user => { return user.id == userId }).shift()
      // explicit is better than implicit :)
      this.form.id = userId
      this.form.name = editedUser.name
      this.form.surname = editedUser.surname
      this.form.dateOfBirth = editedUser.dateOfBirth
      this.form.mobile = editedUser.mobile
      this.form.address = editedUser.address
    },
    saveUser(userId) {
      axios.patch('http://localhost:3000/users/' + userId, this.form)
      .then(response => {
        this.getUsers()
      })
      .catch(errors => { 
        console.log(errors)
      })
    },
    deleteUser(userId) {
      axios.delete('http://localhost:3000/users/' + userId)
      .then(response => { 
        this.users = this.users.filter(user => { return user.id != userId })
      })
      .catch(errors => { 
        console.log(errors)
      })
    }
  },
  created() {
    // fetch data when component is created
    this.getUsers()
  }
}
</script>

<style lang="stylus">
  .app
    table
      margin 10vh auto
      tr
        th
        td
          padding 0 2vw
          text-align left

    form
      text-align center
</style>