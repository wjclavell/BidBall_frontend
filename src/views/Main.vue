<template>
  <div class="main">
    <button
      class="button is-primary"
      style="background-color: #7bc473"
      @click="sampleData"
    >
      Show the events!
    </button>
    <!-- WILL MAKE A TEMPLATE CARD AND LOOP THROUGH EACH EVENT TO CREATE THE CARDS FOR ALL DATA FROM REQUEST -->
    <div class="event-container">
      <div class="game-card" v-for="game in events" :key="game">
        <div class="teams">
          <div class="team">
            <p>Away</p>
            <h4>{{ game.team1 }}</h4>
          </div>
          <div class="team">
            <p>Home</p>
            <h4>{{ game.team2 }}</h4>
          </div>
        </div>
        <div class="scores">
          <h2>{{ game.score1 }}</h2>
          <h2>{{ game.score2 }}</h2>
        </div>
        <div class="bid-input">
          <b-field v-if="game.status === 'STATUS_SCHEDULED'">
            <p class="control">
              <span class="button is-static coin-holder"
                ><img id="game-bit" src="../assets/bidball_greenemblem.png"
              /></span>
            </p>
            <b-input type="number" placeholder="Amount"></b-input>
          </b-field>
          <b-field v-if="game.status === 'STATUS_SCHEDULED'">
            <p class="control">
              <button
                class="button"
                style="background-color: #7bc473; color: white;"
              >
                <strong>Place Bid</strong>
              </button>
            </p>
          </b-field>
          <h1 v-if="game.status != 'STATUS_SCHEDULED'">Game in progress</h1>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
require("dotenv").config();
const unirest = require("unirest"); // unirest library used to make requests to 'the rundown' api
const KEY = process.env.VUE_APP_API_KEY;

//sample data
let events = [
  {
    event_id: "cfcb881774035cb5da1a01bcd1464b74",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "cfcb881774035cb5da1a01bcd1464b74",
      event_status: "STATUS_SCHEDULED",
      score_away: 0,
      score_home: 0,
      winner_away: 0,
      winner_home: 0,
      score_away_by_period: [],
      score_home_by_period: [],
      venue_name: "Marlins Park",
      venue_location: "Miami, Florida",
      game_clock: 0,
      display_clock: "0.00",
      game_period: 1,
      broadcast: "",
      event_status_detail: "9/17 - 1:10 PM EDT",
    },
    teams: [
      {
        team_id: 31134,
        team_normalized_id: 47,
        name: "Boston Red Sox",
        is_away: true,
        is_home: false,
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Miami Marlins",
        is_away: false,
        is_home: true,
      },
    ],
  },
  {
    event_id: "f89fcb881a1a01bcd1464b74",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "f89fcb881a1a01bcd1464b74",
      event_status: "STATUS_FINAL",
      score_away: 3,
      score_home: 2,
      winner_away: 1,
      winner_home: 0,
      score_away_by_period: [],
      score_home_by_period: [],
      venue_name: "Orioles Land",
      venue_location: "Miami, Florida",
      game_clock: 0,
      display_clock: "0.00",
      game_period: 1,
      broadcast: "",
      event_status_detail: "9/17 - 1:10 PM EDT",
    },
    teams: [
      {
        team_id: 31134,
        team_normalized_id: 47,
        name: "New York Yankees",
        is_away: true,
        is_home: false,
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Baltimore Orioles",
        is_away: false,
        is_home: true,
      },
    ],
  },
  {
    event_id: "f89fcb881a1a01bcd1464b74",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "f89fcb881a1a01bcd1464b74",
      event_status: "STATUS_IN_PROGRESS",
      score_away: 1,
      score_home: 4,
      winner_away: 0,
      winner_home: 0,
      score_away_by_period: [],
      score_home_by_period: [],
      venue_name: "Dodger Stadium",
      venue_location: "Miami, Florida",
      game_clock: 0,
      display_clock: "0.00",
      game_period: 1,
      broadcast: "",
      event_status_detail: "9/17 - 1:10 PM EDT",
    },
    teams: [
      {
        team_id: 31134,
        team_normalized_id: 47,
        name: "Los Angeles Angels",
        is_away: true,
        is_home: false,
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Los Angeles Dodgers",
        is_away: false,
        is_home: true,
      },
    ],
  },
  {
    event_id: "cfcb881774035cb5da1a01bcd1464b74",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "cfcb881774035cb5da1a01bcd1464b74",
      event_status: "STATUS_SCHEDULED",
      score_away: 0,
      score_home: 0,
      winner_away: 0,
      winner_home: 0,
      score_away_by_period: [],
      score_home_by_period: [],
      venue_name: "Marlins Park",
      venue_location: "Miami, Florida",
      game_clock: 0,
      display_clock: "0.00",
      game_period: 1,
      broadcast: "",
      event_status_detail: "9/17 - 1:10 PM EDT",
    },
    teams: [
      {
        team_id: 31134,
        team_normalized_id: 47,
        name: "New York Mets",
        is_away: true,
        is_home: false,
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Philadelphia Phillies",
        is_away: false,
        is_home: true,
      },
    ],
  },
];

export default {
  name: "Main",
  components: {},
  props: ["token"],
  data: function() {
    return {
      URL: "http://localhost:8000/",
      rundown: "https://therundown-therundown-v1.p.rapidapi.com/",
      pick: "away",
      date: new Date() //convert today's date to yyyy-mm-dd format, used to get today's games
        .toJSON()
        .slice(0, 10)
        .replace(/-/g, "/"),
      sport_id: null, // sport_id will be determined by what category the user clicks on
      events: [],
      team1: "",
      team2: "",
      score1: "",
      score2: "",
    };
  },
  methods: {
    // method to get all the daily games/events by sport
    getEvents: function() {
      //parameters after 'date' are optional
      //* /sports/{sport_id}/events/{yyyy-mm-dd}?include={periods}&include={scores}&offset={240}

      let req = unirest(
        "GET",
        `${this.rundown}/sports/${this.sport_id}/events/${this.date}`
      );

      req.query({
        include: ["full-game", "scores"],
        offset: "240",
      });

      req.headers({
        "x-rapidapi-host": "therundown-therundown-v1.p.rapidapi.com",
        "x-rapidapi-key": KEY,
        useQueryString: true,
      });

      req.end(function(res) {
        if (res.error) throw new Error(res.error);
        // TODO this is where game/event cards will be created using data from request
        // stuff below was just testing if I could get and use the data
        console.log(res.body);
        console.log("event ID: ", res.body.events[0].event_id);
        console.log(
          "teams playing: ",
          "Away: ",
          // first team wil always be the away team
          res.body.events[0].teams[0].name, //team name
          res.body.events[0].score.score_away, //team score
          "Home: ",
          res.body.events[0].teams[1].name,
          res.body.events[0].score.score_home
        );
        console.log(this.winner);
        // if (
        //   this.winner == "away" &&
        //   res.body.events[0].score.winner_away == 1
        // ) {
        //   console.log("RESULT: you win!");
        // } else if (
        //   this.winner == "home" &&
        //   res.body.events[0].score.winner_home == 1
        // ) {
        //   console.log("RESULT: you win!");
        // } else {
        //   console.log("RESULT: you lose!");
        // }
      });
    },
    getSports: function() {
      let req = unirest(
        "GET",
        "https://therundown-therundown-v1.p.rapidapi.com/sports"
      );

      req.headers({
        "x-rapidapi-host": "therundown-therundown-v1.p.rapidapi.com",
        "x-rapidapi-key": KEY,
        useQueryString: true,
      });

      req.end(function(res) {
        if (res.error) throw new Error(res.error);

        console.log(res.body);
      });
    },
    dataWorking: function() {
      // checking if variables are correct
      console.log(this.pick, this.date);
    },
    sampleData: function() {
      events.forEach((game) => {
        this.events.push({
          team1: game.teams[0].name,
          team2: game.teams[1].name,
          score1: game.score.score_away,
          score2: game.score.score_home,
          status: game.score.event_status,
        });
      });
    },
  },
};
</script>
<style>
.main {
  height: 100vh;
}
.event-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 1em;
  height: 300px;
}
.game-card {
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 0.5em;
  width: 225px;
  border-radius: 25px;
  margin: 1em;
  box-shadow: 0 4px 7px #b1b1b1;
}
.teams,
.scores {
  display: flex;
  justify-content: space-evenly;
  margin: 1em;
}
.scores {
  font-size: 2em;
  margin: 0 1em;
  color: #812286;
}
.bid-input {
  margin: 1em 0;
}
.coin-holder {
  width: 5em;
}
#game-bit {
  height: 2em;
  width: 2em;
}
</style>
