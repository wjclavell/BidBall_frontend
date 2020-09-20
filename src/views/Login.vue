<template>
  <div id="login-page">
    <div class="login-form">
      <b-field class="lfields" label="Username" label-position="on-border">
        <b-input v-model="username" maxlength="30"></b-input>
      </b-field>
      <b-field class="lfields" label="Password" label-position="on-border">
        <b-input v-model="password" type="password" password-reveal></b-input>
      </b-field>
      <a class="button is-primary" style="background-color: #7bc473">
        <!-- <router-link style="color: white; background-color: transparent" to="/"> -->
        <strong id="signin" @click="handleLogin">Login</strong>
        <!-- </router-link> -->
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  props: ["url"],
  data: function() {
    return {
      username: "",
      password: "",
      URL: this.url,
    };
  },
  methods: {
    handleLogin: function() {
      fetch(`${this.URL}auth/users/login/`, {
        method: "post",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          username: this.username,
          password: this.password,
        }),
      })
        .then((response) => {
          if (response.status !== 200) {
            //handle incorrect login
            response.json();
          } else {
            return response.json();
          }
        })
        .then((data) => {
          if (data) {
            this.$emit("loggedIn", data);
          } else {
            this.$buefy.toast.open({
              message: "Username or password is incorrect",
              type: "is-danger",
              duration: 2000,
            });
          }
        });
    },
  },
};
</script>

<style>
#login-page {
  height: 100vh;
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
  min-width: 300px;
  background-color: white;
  border-radius: 1em;
  box-shadow: 0 4px 7px #b1b1b1;
}
.lfields {
  align-self: center;
  width: 90%;
  margin: 1em;
}
.login-form a {
  display: flex;
  justify-content: center;
  align-self: center;
  width: 25%;
}
#signin:hover {
  color: #812286;
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
  .lfields {
    align-self: center;
    width: 90%;
    margin: 0.5em;
  }
  .login-form a {
    width: 50%;
  }
}
</style>
