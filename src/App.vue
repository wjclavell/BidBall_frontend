<template>
  <div id="app">
    <div id="nav">
      <Header v-bind:URL="URL" v-bind:loggedIn="loggedIn" @logout="logout" />
    </div>
    <section>
      <router-view @loggedIn="login($event)" />
    </section>
    <Footer />
  </div>
</template>

<script>
import Header from "./components/Header";
import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Header,
    Footer,
  },
  data: function () {
    return {
      loggedIn: false,
      token: "",
      URL: "http://localhost:8000/",
    };
  },
  methods: {
    login: function (event) {
      console.log("event heard: ");
      this.loggedIn = true;
      this.token = event.token;
      console.log(this.token);
      this.$router.push("/main");
    },
    logout: function () {
      this.loggedIn = false;
      this.token = "";
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #f5f5f5;
}

#nav {
  margin: 1em 0 2em 0;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #7bc473;
}
</style>
