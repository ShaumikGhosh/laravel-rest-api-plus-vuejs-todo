<template>
  <div>
    <Header/>
    <div class="form-area">
      <h1>Register form</h1>
      <b-form @submit.prevent="onFormSubmit">
        <b-form-group
          id="input-group-1"
          label="Email address:"
          label-for="input-1"
        >
          <b-form-input
            id="input-1"
            type="email"
            placeholder="Enter your email"
            required
            v-model="email"
          ></b-form-input>
          <p style="color:red">{{mail_error}}</p>
        </b-form-group>

        <b-form-group id="input-group-2" label="Password:" label-for="input-2">
          <b-form-input
            id="input-2"
            placeholder="Enter your password"
            required
            type="password"
            v-model="password"
          ></b-form-input>
        </b-form-group>
        <b-form-group
          id="input-group-2"
          label="Confirm Password:"
          label-for="input-2"
        >
          <b-form-input
            id="input-3"
            placeholder="Confirm your password"
            required
            type="password"
            v-model="confirm_password"
          ></b-form-input>
          <p style="color:red">{{password_error}}</p>
        </b-form-group>
        <b-button type="submit" variant="primary">Register now</b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
import Header from '../components/Header'

export default {

  name: "Register",

    components : {
      Header
    },
  
  data() {
    return {
      email: null,
      password: null,
      confirm_password: null,
      mail_error: null,
      password_error: null,
    };
  },
  
  methods: {
    onFormSubmit() {
      const data = {
        email: this.email,
        password: this.password,
        password_confirmation: this.confirm_password,
      };

      const headers = {
        "Content-Type": "application/json;charset=UTF-8",
        "Access-Control-Allow-Origin": "*",
        "Accept": "application/json",
      };

      this.$axios
        .post(`${this.$base_path}/api/register`, data, {
          headers: headers,
        })
        .then((response) => {
          if(response.data.email)
          {
            this.mail_error = response.data.email[0];
          }
          if(response.data.password)
          {
            this.password_error = response.data.password[0];
          }
          if(response.data.status === 'success')
          {
            this.$router.push(`login?message=${response.data.message}`);
          }


        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style scoped>
.form-area {
  width: 400px;
  border: 1px solid black;
  margin: 100px auto;
  padding: 20px;
}
</style>