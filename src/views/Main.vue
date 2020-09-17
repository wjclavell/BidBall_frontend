<template>
  <div class="main">
    <div class="categories">
      <div class="sport-wrap">
        <h1 class="labels">
          Baseball
        </h1>
        <b-icon
          @click.native="pickSport"
          class="ball"
          id="3"
          pack="fas"
          icon="baseball-ball"
          size="is-large"
        ></b-icon>
      </div>
      <div class="sport-wrap">
        <h1 class="labels">
          Basketball
        </h1>
        <b-icon
          @click.native="pickSport"
          class="ball"
          id="4"
          pack="fas"
          icon="basketball-ball"
          size="is-large"
        ></b-icon>
      </div>
      <div class="sport-wrap">
        <h1 class="labels">
          Football
        </h1>
        <b-icon
          @click.native="pickSport"
          class="ball"
          id="2"
          pack="fas"
          icon="football-ball"
          size="is-large"
        ></b-icon>
      </div>
    </div>
    <button
      class="button is-primary"
      style="background-color: #7bc473"
      @click="sampleData"
    >
      Show the events!
    </button>
    <!-- WILL MAKE A TEMPLATE CARD AND LOOP THROUGH EACH EVENT TO CREATE THE CARDS FOR ALL DATA FROM REQUEST -->
    <div class="event-container">
      <div class="game-card" v-for="game in this.events" :key="game">
        <div class="teams">
          <div class="team">
            <h4>{{ game.team1 }}</h4>
          </div>
          <div class="team">
            <h4>{{ game.team2 }}</h4>
          </div>
        </div>
        <div class="scores">
          <h2>{{ game.score1 }}</h2>
          <h2>{{ game.score2 }}</h2>
        </div>
        <div class="bid-input">
          <div v-if="game.status === 'STATUS_SCHEDULED'" class="block">
            <b-radio v-model="pick" native-value="away">
              {{ game.team1_abbrev }}
            </b-radio>
            <b-radio v-model="pick" native-value="home">
              {{ game.team2_abbrev }}
            </b-radio>
          </div>
          <b-field v-if="game.status === 'STATUS_SCHEDULED'">
            <p class="control">
              <span class="button is-static coin-holder"
                ><img id="game-bit" src="../assets/bidball_greenemblem.png"
              /></span>
            </p>
            <b-input
              v-model="amount"
              type="number"
              placeholder="Amount"
            ></b-input>
          </b-field>
          <b-field v-if="game.status === 'STATUS_SCHEDULED'">
            <p class="control">
              <button
                @click="placeBid(game.event_id)"
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
let juegos = [
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
        abbreviation: "BOS",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Miami Marlins",
        is_away: false,
        is_home: true,
        abbreviation: "MIA",
      },
    ],
  },
  {
    event_id: "f89fcb881a1a01bcd1464t",
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
        abbreviation: "NYY",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Baltimore Orioles",
        is_away: false,
        is_home: true,
        abbreviation: "BAL",
      },
    ],
  },
  {
    event_id: "122f89fcb881a1a01bcd1464poop",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "122f89fcb881a1a01bcd1464b74poop",
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
        abbreviation: "LAA",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Los Angeles Dodgers",
        is_away: false,
        is_home: true,
        abbreviation: "LAD",
      },
    ],
  },
  {
    event_id: "redb881774035cb5da1a01bcd1464b74one",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "redcfcb881774035cb5da1a01bcd1464b74one",
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
        abbreviation: "NYM",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Philadelphia Phillies",
        is_away: false,
        is_home: true,
        abbreviation: "PHI",
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
        name: "Boston Red Sox",
        is_away: true,
        is_home: false,
        abbreviation: "BOS",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Miami Marlins",
        is_away: false,
        is_home: true,
        abbreviation: "MIA",
      },
    ],
  },
  {
    event_id: "f89fcb881a1a01bcd1464t",
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
        abbreviation: "NYY",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Baltimore Orioles",
        is_away: false,
        is_home: true,
        abbreviation: "BAL",
      },
    ],
  },
  {
    event_id: "122f89fcb881a1a01bcd1464poop",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "122f89fcb881a1a01bcd1464b74poop",
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
        abbreviation: "LAA",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Los Angeles Dodgers",
        is_away: false,
        is_home: true,
        abbreviation: "LAD",
      },
    ],
  },
  {
    event_id: "redb881774035cb5da1a01bcd1464b74one",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "redcfcb881774035cb5da1a01bcd1464b74one",
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
        abbreviation: "NYM",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Philadelphia Phillies",
        is_away: false,
        is_home: true,
        abbreviation: "PHI",
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
        name: "Boston Red Sox",
        is_away: true,
        is_home: false,
        abbreviation: "BOS",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Miami Marlins",
        is_away: false,
        is_home: true,
        abbreviation: "MIA",
      },
    ],
  },
  {
    event_id: "f89fcb881a1a01bcd1464t",
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
        abbreviation: "NYY",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Baltimore Orioles",
        is_away: false,
        is_home: true,
        abbreviation: "BAL",
      },
    ],
  },
  {
    event_id: "122f89fcb881a1a01bcd1464poop",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "122f89fcb881a1a01bcd1464b74poop",
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
        abbreviation: "LAA",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Los Angeles Dodgers",
        is_away: false,
        is_home: true,
        abbreviation: "LAD",
      },
    ],
  },
  {
    event_id: "redb881774035cb5da1a01bcd1464b74one",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "redcfcb881774035cb5da1a01bcd1464b74one",
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
        abbreviation: "NYM",
      },
      {
        team_id: 31210,
        team_normalized_id: 32,
        name: "Philadelphia Phillies",
        is_away: false,
        is_home: true,
        abbreviation: "PHI",
      },
    ],
  },
];

export default {
  name: "Main",
  props: ["token", "url"],
  data: function() {
    return {
      BBurl: this.url, //url for local api
      rundown: "https://therundown-therundown-v1.p.rapidapi.com", //url for external api
      pick: "",
      event_id: "",
      amount: null,
      date: new Date() //convert today's date to yyyy-mm-dd format, used to get today's games
        .toJSON()
        .slice(0, 10)
        .replace(/-/g, "-"),
      sport_id: null, // sport_id will be determined by what category the user clicks on
      events: [],
      // team1: "",
      // team2: "",
      // score1: "",
      // score2: "",
      user_token: this.token,
    };
  },
  computed: {},
  methods: {
    // method to get all the daily games/events by sport
    getEvents: function() {
      //parameters after 'date' are optional
      //* /sports/{sport_id}/events/{yyyy-mm-dd}?include={periods}&include={scores}&offset={240}
      this.events = []; //empty the array to get rid of any previous games
      let event_list = this.events;
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
        //! something is wrong...i believe the cards are being created from the global array (which is empty) could work with global array but i dont think it is re-rendering the card-container when new data is added
        //! other note, for some reason i can't access 'this'
        // stuff below was just testing if I could get and use the data
        console.log(res.body);
        console.log("event ID: ", res.body.events[0].event_id);
        console.log(
          "teams playing: ",
          "Away:",
          // first team wil always be the away team
          res.body.events[0].teams[0].name, //team name
          res.body.events[0].score.score_away, //team score
          " Home:",
          res.body.events[0].teams[1].name,
          res.body.events[0].score.score_home
        );

        // event_list = []; //empty the events array
        res.body.events.forEach((game) => {
          //for each game in the request data
          event_list.push({
            //push relevant info to array to use for cards
            event_id: game.event_id,
            team1: game.teams[0].name,
            team2: game.teams[1].name,
            team1_abbrev: game.teams_normalized[0].abbreviation,
            team2_abbrev: game.teams_normalized[1].abbreviation,
            score1: game.score.score_away,
            score2: game.score.score_home,
            status: game.score.event_status,
          });
        });
        console.log(event_list);
        return event_list;
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
      this.events = [];
      juegos.forEach((game) => {
        this.events.push({
          event_id: game.event_id,
          team1: game.teams[0].name,
          team2: game.teams[1].name,
          team1_abbrev: game.teams[0].abbreviation,
          team2_abbrev: game.teams[1].abbreviation,
          score1: game.score.score_away,
          score2: game.score.score_home,
          status: game.score.event_status,
        });
      });
      console.log(this.events);
    },
    pickSport: function() {
      this.sport_id = event.target.id;
      this.sampleData();
    },
    //TODO this method will create a bid for the user based on the inputted pick, amount, and event_id
    placeBid: function(id) {
      this.event_id = id;
      console.log(`bid pick: ${this.pick}`);
      console.log(`bid amount: ${this.amount}`);
      console.log(`event ID: ${this.event_id}`);
    },
  },
};
</script>
<style>
.main {
  height: 100vh;
}
.categories {
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-top: 2px solid #b1b1b1;
  border-bottom: 2px solid #b1b1b1;
  margin-bottom: 3em;
}
.sport-wrap {
  display: flex;
  align-items: center;
  padding: 0.5em;
}
.labels {
  font-size: 2em;
  margin-right: 1em;
}
.ball {
  color: #b1b1b1;
}
.ball:hover {
  color: #50b963;
  cursor: pointer;
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
  border-radius: 2em;
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
