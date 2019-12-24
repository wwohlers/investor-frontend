<template>
  <div class="about">
    <h1>Sign up</h1>
    <form @submit.prevent="signup()">
      <input type="text" v-model="name" placeholder="Name" />
      <input type="text" v-model="email" placeholder="Email" />
      <input type="password" v-model="password" />
      <button>Sign up</button>
    </form>
  </div>
</template>

<script>
const Axios = require('axios');
const UserApi = "http://localhost:3000/users";

export default {
  data() {
    return {
      name: '',
      email: '',
      password: '',
    };
  },
  methods: {
    signup() {
      let self = this;
      const nameInput = this.name;
      const emailInput = this.email;
      const passInput = this.password;
      Axios.post(UserApi, {
        name: nameInput,
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