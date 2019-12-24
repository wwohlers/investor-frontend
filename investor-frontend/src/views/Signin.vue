<template>
  <div class="about">
    <h1>Sign in</h1>
    <form @submit.prevent="login()">
      <input type="text" v-model="email" placeholder="Email" />
      <input type="password" v-model="password" />
      <button>Sign in</button>
    </form>
  </div>
</template>

<script>
const Axios = require('axios');
const UserApi = "http://localhost:3000/users";

export default {
  data() {
    return {
      email: '',
      password: '',
    };
  },
  methods: {
    login() {
      let self = this;
      const emailInput = this.email;
      const passInput = this.password;
      Axios.post(UserApi + '/login', {
        email: emailInput,
        password: passInput
      })
      .then(function (response) {
        if (response.data.user) {
          self.$cookie.set('token', response.data.token);
          self.$router.push('dash');
        } else {
          console.log("Failed");
        }
      })
      .catch(function (error) {
        console.log(error);
      });
    }
  }
}
</script>