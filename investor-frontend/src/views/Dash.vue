<template>
  <div class="about">
    <h1>Dashboard</h1>
    <button @click="logout()">Logout</button>
    <button @click="logoutAll()">Logout All</button>
    <p>{{ info }}</p>
  </div>
</template>

<script>
const Axios = require('axios');
const UserApi = "http://localhost:3000/users";

export default {
  data() {
    return {
      info: '',
    };
  },
  methods: {
    logout() {
      let self = this;
      var token = this.$cookie.get('token');
      if (!token) {
        self.$router.push('signin');
      } else {
        Axios.post(UserApi + '/me/logout')
        .then(() => {
          self.$cookie.delete('token');
          self.$router.push('signin');
        })
        .catch(function (error) {
          console.log(error);
        })
      }
    },
    logoutAll() {
      let self = this;
      var token = this.$cookie.get('token');
      if (!token) {
        self.$router.push('signin');
      } else {
        Axios.post(UserApi + '/me/logoutall')
        .then(() => {
          self.$cookie.delete('token');
          self.$router.push('signin');
        })
        .catch(function (error) {
          console.log(error);
        })
      }
    }
  },
  mounted() {
    var token = this.$cookie.get('token');
    if (!token) {
      this.$router.push('signin');
    }
    Axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;

    let self = this;
    Axios.get(UserApi + '/me')
    .then(function(response) {
      self.info = response.data.name;
    })
    .catch(function(error) {
      console.log(error);
    })
  }
}
</script>