<template>
  <div id="login-page">
    <div class="login-form">
      <b-field class="fields" label="Username" label-position="on-border">
        <b-input v-model="username" maxlength="30"></b-input>
      </b-field>
      <b-field class="fields" label="Password" label-position="on-border">
        <b-input
          v-model="password"
          message="Password must be atleast 8 characters"
          type="password"
          minlength="8"
        ></b-input>
      </b-field>
      <a class="button is-primary" style="background-color: #812286">
        <router-link style="color: white; background-color: transparent" to="/">
          <strong id="signup">Sign Up</strong>
        </router-link>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data: function () {
    return {
      username: "",
      password: "",
    };
  },
};
</script>

<style>
#login-page {
  height: 70vh;
}
.login-form {
  padding: 2em;
  display: flex;
  flex-direction: column;
  align-content: center;
  position: relative;
  top: 5em;
  margin: 0 auto;
  width: 60vw;
  max-width: 500px;
  background-color: white;
  border-radius: 1em;
  box-shadow: 0 4px 7px #b1b1b1;
}
.fields {
  align-self: center;
  width: 70%;
  margin: 1em;
}
.login-form a {
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
@media only screen and (max-width: 400px) {
  .fields {
    align-self: center;
    width: 90%;
    margin: 0.5em;
  }
  .login-form a {
    width: 50%;
  }
}
</style>