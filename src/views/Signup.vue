<template>
  <div id="signup-page">
    <h1>Sign up and start placing your bids!</h1>
    <div class="signup-form">
      <b-field horizontal label="Name">
        <b-field class="sfields" label="First name" label-position="on-border">
          <b-input v-model="firstName"></b-input>
        </b-field>
        <b-field class="sfields" label="Last name" label-position="on-border">
          <b-input v-model="lastName"></b-input>
        </b-field>
      </b-field>
      <b-field class="sfields" label="Email" label-position="on-border">
        <b-input v-model="email" type="email" maxlength="30"></b-input>
      </b-field>
      <b-field class="sfields" label="Username" label-position="on-border">
        <b-input v-model="username" maxlength="30"></b-input>
      </b-field>
      <b-field class="sfields" label="Password" label-position="on-border">
        <b-input
          v-model="password"
          message="Password must be atleast 8 characters"
          type="password"
          minlength="8"
        ></b-input>
      </b-field>
      <b-field label="Favorite Sport">
        <b-select v-model="favorite_league" placeholder="Favorite Sport">
          <option>Baseball</option>
          <option>Basketball</option>
          <option>Football</option>
        </b-select>
      </b-field>
      <a class="button is-primary signup-btn" style="background-color: #812286">
        <router-link style="color: white; background-color: transparent" to="/">
          <strong id="signup" @click="handleRegister">Sign Up</strong>
        </router-link>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Signup",
  props: ["url"],
  data: function() {
    return {
      firstName: "",
      lastName: "",
      email: "",
      username: "",
      password: "",
      favorite_league: "",
      URL: this.url,
    };
  },
  methods: {
    handleRegister: function() {
      fetch(`${this.URL}auth/users/register/`, {
        method: "post",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          username: this.username,
          password: this.password,
          email: this.email,
          firstName: this.firstName,
          lastName: this.lastName,
          favorite_league: this.favorite_league,
        }),
      })
        .then((response) => {
          response.json();
        })
        .then((data) => {
          console.log(data);
          alert("Sign up successful! Now login in to continue.");
        });
    },
  },
};
</script>

<style>
#signup-page {
  height: 100vh;
}
.signup-form {
  padding: 2em;
  display: flex;
  flex-direction: column;
  align-content: center;
  position: relative;
  top: 2em;
  margin: 0 auto;
  width: 60vw;
  max-width: 500px;
  min-width: 300px;
  background-color: white;
  border-radius: 1em;
  box-shadow: 0 4px 7px #b1b1b1;
}
.sfields {
  align-self: center;
  width: 90%;
}
.signup-form a {
  display: flex;
  justify-content: center;
  align-self: center;
  width: 25%;
}
#signup:hover {
  animation: pulse ease-in-out 1s infinite;
}
@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.2);
  }
  100% {
    transform: scale(1);
  }
}
@media only screen and (min-width: 400px) {
  .sfields {
    align-self: center;
    margin: 0.5em;
  }
  .login-form {
    width: 90vw;
  }
  .login-form a {
    width: 100%;
  }
  .signup-btn {
    width: 200px;
  }
}
</style>
