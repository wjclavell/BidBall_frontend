<template>
  <div id="app">
    <div id="page-container">
      <div id="wrap">
        <div id="nav">
          <Header
            v-bind:url="URL"
            v-bind:loggedIn="loggedIn"
            v-bind:user="user"
            @logout="logout"
          />
        </div>
        <router-view
          @loggedIn="login($event)"
          @registered="signup($event)"
          @logout="logout"
          @reassign="reassign($event)"
          :url="URL"
          :user="user"
        />
      </div>
      <footer id="footer">
        <div class="content has-text-centered">
          <p>
            <strong style="color: #812286">Bid ball</strong> © 2020 William
            Clavell |
            <a href="https://wjclavell.com">Portfolio |</a>
            <a href="http://opensource.org/licenses/mit-license.php"> Github </a
            >|
          </p>
        </div>
      </footer>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
// import Footer from "./components/Footer";

export default {
  name: "App",
  components: {
    Header,
    // Footer,
  },
  data: function() {
    return {
      loggedIn: false,
      URL: "https://project-cuatro.herokuapp.com/",
      // URL: "http://localhost:8000/",
      user: {},
      sport_id: null,
    };
  },
  methods: {
    login: function(event) {
      this.loggedIn = true;
      this.user = event;
      // this.$cookies.set("user", this.user);
      localStorage.setItem("token", this.user.token);
      localStorage.setItem("username", this.user.username);
      localStorage.setItem("balance", this.user.balance);
      localStorage.setItem("favoriteSport", this.user.favorite_league);
      localStorage.setItem("favoriteTeams", this.user.favorite_teams);
      localStorage.setItem("profilePic", this.user.profile_pic);
      this.$router.push("/main");
    },
    logout: function() {
      this.loggedIn = false;
      this.user = "";
      // this.$cookies.remove("user");
      localStorage.clear();
      // this.$router.push("/");
    },
    signup: function(event) {
      this.loggedIn = true;
      this.user = event;
      this.$router.push("/main");
    },
    reassign: function(event) {
      this.user = event;
    },
  },
  beforeMount: function() {
    this.user.token = localStorage.getItem("token");
    this.user.username = localStorage.getItem("username");
    this.user.balance = localStorage.getItem("balance");
    this.user.favorite_sport = localStorage.getItem("favoriteSport");
    this.user.favorite_teams = localStorage.getItem("favoriteTeams");
    this.user.profile_pic = localStorage.getItem("profilePic");
    if (this.user.token) {
      this.loggedIn = true;
    }
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
  min-height: 100vh;
}

#page-container {
  position: relative;
  min-height: 100vh;
}

#wrap {
  padding-bottom: 2em;
}

#footer {
  position: absolute;
  width: 100%;
  height: 2em;
  bottom: 0;
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
