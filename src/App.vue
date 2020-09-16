<template>
  <div id="app">
    <div id="nav">
      <Header />
    </div>
    <router-view @loggedIn="login($event)" />
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
      loggedin: false,
      token: "",
    };
  },
  methods: {
    login: function (event) {
      console.log("event heard: ");
      this.loggedin = true;
      this.token = event.token;
      console.log(this.token);
      this.$router.push("/");
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

router-view {
  height: 100vh;
}
</style>
