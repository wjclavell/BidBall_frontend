<template>
  <div id="app">
    <div id="page-container">
      <div id="wrap">
        <div id="nav">
          <Header
            v-bind:url="URL"
            v-bind:loggedIn="loggedIn"
            :user="user"
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
const unirest = require("unirest"); // unirest library used to make requests to 'the rundown' api
const KEY = process.env.VUE_APP_API_KEY;

export default {
  name: "App",
  components: {
    Header,
    // Footer,
  },
  data: function () {
    return {
      loggedIn: false,
      URL: "https://project-cuatro.herokuapp.com/",
      // URL: "http://localhost:8000/",
      user: {},
      sport_id: null,
      no_console: null,
    };
  },
  methods: {
    login: function (event) {
      this.loggedIn = true;
      this.user = event;
      // this.$cookies.set("user", this.user);
      localStorage.setItem("token", this.user.token);
      localStorage.setItem("username", this.user.username);
      localStorage.setItem("balance", this.user.balance);
      localStorage.setItem("favoriteSport", this.user.favorite_league);
      localStorage.setItem("favoriteTeams", this.user.favorite_teams);
      localStorage.setItem("profilePic", this.user.profile_pic);
      localStorage.setItem("correct", this.user.correct);
      localStorage.setItem("incorrect", this.user.incorrect);
      this.$router.push("/main");
      /* //TODO to retrieve & check results of a user's bid: 
      1) On login, get all user bids where status === "pending"  (make this route in backend?)
      2) For each bid, fetch for corresponding event in 'the rundown' api using event_id
      3) IF status === STATUS_FINAL then its time to check results! ELSE do nothing
      4) IF bid.pick === 'away' AND event.away_win === true
      5) user.balance += (2 * bid.amount); user.correct += 1; bid.status = "W"
      6) Same if pick is home & win is home
      7) ELSE user.incorrect += 1; bid.status = "L"
      */
      let correctPicks = 0; // keep track of how many picks the user got correct
      let coinsGained = 0; // keep track of how many coins the user has gained from correct picks
      fetch(`${this.URL}api/bids/`, {
        method: "GET",
        headers: {
          "Content-Type": "application/json",
          // only if logged in
          Authorization: `JWT ${this.user.token}`,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          data.results.forEach((bid) => {
            if (bid.result === "pending") {
              const req = unirest(
                "GET",
                `https://therundown-therundown-v1.p.rapidapi.com/events/${bid.event_id}`
              );

              req.query({
                include: "scores",
              });

              req.headers({
                "x-rapidapi-host": "therundown-therundown-v1.p.rapidapi.com",
                "x-rapidapi-key": KEY,
                useQueryString: true,
              });

              req.end(function (res) {
                if (res.error) throw new Error(res.error);

                if (res.body.event_status_detail === "Final") {
                  if (
                    (bid.pick === "away" && res.body.score.winner_away === 1) ||
                    (bid.pick === "home" && res.body.score.winner_home === 1)
                  ) {
                    correctPicks += 1; // increase correct counter
                    coinsGained += 2 * bid.amount; // increase coin counter
                    const editBid = JSON.stringify({
                      result: "W",
                    });
                    // create the bid for this game
                    fetch(`${this.URL}api/bids/`, {
                      method: "PUT",
                      headers: {
                        "Content-Type": "application/json",
                        // only if logged in
                        Authorization: `JWT ${this.user.token}`,
                      },
                      body: editBid,
                    })
                      .then((response) => response.json())
                      .then((data) => {
                        this.user.balance += 2 * bid.amount;
                        this.no_console = data;
                        //after placing a bid, change user's balance permanently
                        const editUser = {
                          email: this.user.email,
                          username: this.user.username,
                          correct: this.user.correct + 1,
                          balance: this.user.balance,
                        };
                        fetch(`${this.URL}auth/users/profile/`, {
                          method: "PUT",
                          headers: {
                            "Content-Type": "application/json",
                            Authorization: `JWT ${this.user.token}`,
                          },
                          body: JSON.stringify(editUser),
                        })
                          .then((response) => response.json())
                          .then((data) => {
                            this.no_console = data;
                          });
                      });
                  } else {
                    const editBid = JSON.stringify({
                      result: "L",
                    });
                    // create the bid for this game
                    fetch(`${this.URL}api/bids/`, {
                      method: "PUT",
                      headers: {
                        "Content-Type": "application/json",
                        // only if logged in
                        Authorization: `JWT ${this.user.token}`,
                      },
                      body: editBid,
                    })
                      .then((response) => response.json())
                      .then((data) => {
                        this.no_console = data;
                        //after placing a bid, change user's balance permanently
                        const editUser = {
                          email: this.user.email,
                          username: this.user.username,
                          incorrect: this.user.incorrect + 1,
                        };
                        fetch(`${this.URL}auth/users/profile/`, {
                          method: "PUT",
                          headers: {
                            "Content-Type": "application/json",
                            Authorization: `JWT ${this.user.token}`,
                          },
                          body: JSON.stringify(editUser),
                        })
                          .then((response) => response.json())
                          .then((data) => {
                            this.no_console = data;
                          });
                      });
                  }
                }
              });
            }
          });
          this.$buefy.toast.open({
            message: `You've had ${correctPicks} correct picks since your last login. ${coinsGained} coins have been added to your account!`,
            type: "is-success",
            duration: 6000,
          });
        });
    },
    logout: function () {
      this.loggedIn = false;
      this.user = "";
      // this.$cookies.remove("user");
      localStorage.clear();
      // this.$router.push("/");
    },
    signup: function (event) {
      this.loggedIn = true;
      this.user = event;
      this.$router.push("/main");
    },
    reassign: function (event) {
      this.user = event;
    },
  },
  beforeMount: function () {
    this.user.token = localStorage.getItem("token");
    this.user.username = localStorage.getItem("username");
    this.user.balance = localStorage.getItem("balance");
    this.user.favorite_sport = localStorage.getItem("favoriteSport");
    this.user.favorite_teams = localStorage.getItem("favoriteTeams");
    this.user.profile_pic = localStorage.getItem("profilePic");
    this.user.correct = localStorage.getItem("correct");
    this.user.incorrect = localStorage.getItem("incorrect");
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
