<template>
  <div class="main">
    <Category @sportID="pickSport($event)" />
    <button
      class="button is-primary"
      style="background-color: #7bc473"
      @click="sampleData"
    >
      Show the events!
    </button>
    <!-- WILL MAKE A TEMPLATE CARD AND LOOP THROUGH EACH EVENT TO CREATE THE CARDS FOR ALL DATA FROM REQUEST -->
    <div v-if="no_games">
      <h1 id="no-games"><strong>NO</strong> <span>GAMES</span> TODAY!</h1>
    </div>
    <VueSlickCarousel
      class="event-container"
      v-if="eventsObtained"
      v-bind="settings"
    >
      <div class="game-card" v-for="item in this.events" :key="item.game">
        <div class="teams">
          <div class="team">
            <h4>{{ item.game.team1 }}</h4>
          </div>
          <div class="team">
            <h4>{{ item.game.team2 }}</h4>
          </div>
        </div>
        <div class="scores">
          <h2>{{ item.game.score1 }}</h2>
          <h2>{{ item.game.score2 }}</h2>
        </div>
        <div class="bid-input">
          <div v-if="item.game.status === 'STATUS_SCHEDULED'" class="block">
            <b-radio v-model="pick" native-value="away">
              {{ item.game.team1_abbrev }}
            </b-radio>
            <b-radio v-model="pick" native-value="home">
              {{ item.game.team2_abbrev }}
            </b-radio>
          </div>
          <b-field v-if="item.game.status === 'STATUS_SCHEDULED'">
            <p class="control">
              <span class="button is-static coin-holder"
                ><img id="game-bit" src="../assets/bidball_greenemblem.png"
              /></span>
            </p>
            <b-input
              v-model="amount"
              type="number"
              min="0"
              placeholder="Amount"
            ></b-input>
          </b-field>
          <b-field v-if="item.game.status === 'STATUS_SCHEDULED'">
            <p class="control">
              <button
                @click="placeBid(item.game)"
                class="button"
                style="background-color: #7bc473; color: white;"
              >
                <strong>Place Bid</strong>
              </button>
            </p>
          </b-field>
          <h1 id="in-progress" v-if="item.game.status === 'STATUS_IN_PROGRESS'">
            Game in progress
          </h1>
          <h1 id="final" v-if="item.game.status === 'STATUS_FINAL'">
            Game Finished
          </h1>
        </div>
      </div>
    </VueSlickCarousel>
  </div>
</template>

<script>
import Category from "../components/Category";
import VueSlickCarousel from "vue-slick-carousel";
import "vue-slick-carousel/dist/vue-slick-carousel.css";
// optional style for arrows & dots
import "vue-slick-carousel/dist/vue-slick-carousel-theme.css";
require("dotenv").config();

const unirest = require("unirest"); // unirest library used to make requests to 'the rundown' api
const KEY = process.env.VUE_APP_API_KEY;

//sample data
let juegos = [
  {
    event_id: "cf81774035cb5da1a01bcd1464b74",
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
    event_id: "f89fcb881a1a0ggg1bcd1464t",
    sport_id: 3,
    event_date: "2020-09-17T17:10:00Z",
    rotation_number_away: 923,
    rotation_number_home: 924,
    score: {
      event_id: "f89fcb881a1a01bcd1464b74",
      event_status: "STATUS_SCHEDULED",
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
      event_status: "STATUS_SCHEDULED",
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
    event_id: "redb881774035cb5da1a01bWAScd1464b74one",
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
  props: ["user", "url"],
  components: { Category, VueSlickCarousel },
  data: function() {
    return {
      URL: this.url, //url for local api
      rundown: "https://therundown-therundown-v1.p.rapidapi.com", //url for external api
      pick: "",
      event_id: "",
      amount: null,
      eventsObtained: false,
      settings: {
        dots: true,
        focusOnSelect: true,
        infinite: true,
        speed: 500,
        slidesToShow: 4,
        slidesToScroll: 4,
        touchThreshold: 5,
        responsive: [
          {
            breakpoint: 1024,
            settings: {
              slidesToShow: 3,
              slidesToScroll: 3,
              infinite: true,
              dots: true,
            },
          },
          {
            breakpoint: 600,
            settings: {
              slidesToShow: 2,
              slidesToScroll: 2,
              initialSlide: 2,
            },
          },
          {
            breakpoint: 480,
            settings: {
              slidesToShow: 1,
              slidesToScroll: 1,
            },
          },
        ],
      },
      date: new Date() //convert today's date to yyyy-mm-dd format, used to get today's games
        .toJSON()
        .slice(0, 10)
        .replace(/-/g, "-"),
      sport_id: null, // sport_id will be determined by what category the user clicks on
      events: [],
      game_id: null, //used to assign bid to created game
      no_games: false, //if there are no games today this will become true and display a message
      // team1: "",
      // team2: "",
      // score1: "",
      // score2: "",
      user_info: this.user,
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
      let self = this;
      // let none = this.no_games;
      let req = unirest(
        "GET",
        `${this.rundown}/sports/${this.sport_id}/events/${this.date}`
      );

      req.query({
        include: ["full-game", "scores"],
        offset: "360",
      });

      req.headers({
        "x-rapidapi-host": "therundown-therundown-v1.p.rapidapi.com",
        "x-rapidapi-key": KEY,
        useQueryString: true,
      });

      req.end(function(res) {
        if (res.error) throw new Error(res.error);
        // stuff below was just testing if I could get and use the data
        console.log(res.body);
        // if (res.body.events.length === 0) {
        //   console.log(none);
        //   none = true;
        //   console.log(none);
        //   return none;
        // }
        // event_list = []; //empty the events array
        res.body.events.forEach((game) => {
          //for each game in the request data
          self.events.push({
            game: {
              //push relevant info to array to use for cards
              event_id: game.event_id,
              sport_id: game.sport_id,
              team1: game.teams[0].name,
              team2: game.teams[1].name,
              team1_abbrev: game.teams_normalized[0].abbreviation,
              team2_abbrev: game.teams_normalized[1].abbreviation,
              score1: game.score.score_away,
              score2: game.score.score_home,
              status: game.score.event_status,
            },
          });
        });
        self.eventsObtained = true;
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
      // this.no_games = none;
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
    sampleData: function() {
      this.events = [];
      juegos.forEach((game) => {
        this.events.push({
          game: {
            event_id: game.event_id,
            sport_id: game.sport_id,
            team1: game.teams[0].name,
            team2: game.teams[1].name,
            team1_abbrev: game.teams[0].abbreviation,
            team2_abbrev: game.teams[1].abbreviation,
            score1: game.score.score_away,
            score2: game.score.score_home,
            status: game.score.event_status,
          },
        });
      });
      this.eventsObtained = true;
      console.log(this.events);
    },
    // when user selects a sport category we will set the id to the sport_id and run the fetch request with the events for that sport
    pickSport: function(id) {
      this.sport_id = id;
      this.getEvents();
    },
    //this method will create a bid for the user based on the inputted pick, amount, and event_id
    placeBid: function(info) {
      if (this.amount > this.user_info.balance / 2) {
        this.$buefy.toast.open({
          message: `You currently have ${this.user_info.balance} coins, you can only bid up to half your current balance on a single game`,
          type: "is-danger",
          duration: 4000,
        });
        // alert(
        //   `You currently have ${this.user_info.balance} coins, you can only bid up to half your current balance on a single game`
        // );
      } else {
        this.event_id = info.event_id;
        console.log(`bid pick: ${this.pick}`);
        console.log(`bid amount: ${this.amount}`);
        console.log(`event ID: ${this.event_id}`);
        console.log(info);
        const newGame = JSON.stringify({
          event_id: info.event_id,
          sport: info.sport_id,
          team1: info.team1,
          team2: info.team2,
          score1: info.score1,
          score2: info.score2,
        });
        // create a game in the db for this game
        fetch(`${this.URL}api/games/`, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            // only if logged in
            Authorization: `JWT ${this.user.token}`,
          },
          body: newGame,
        })
          .then((response) => response.json())
          .then((data) => {
            this.game_id = data.id;
            console.log(`created game: `, data, data.id);

            const newBid = JSON.stringify({
              game: this.game_id,
              event_id: info.event_id,
              amount: this.amount,
              team: this.pick,
              result: "pending",
            });
            // create the bid for this game
            fetch(`${this.URL}api/bids/`, {
              method: "POST",
              headers: {
                "Content-Type": "application/json",
                // only if logged in
                Authorization: `JWT ${this.user.token}`,
              },
              body: newBid,
            })
              .then((response) => response.json())
              .then((data) => {
                this.event_id = "";
                this.user_info.balance -= this.amount;
                console.log(`created bid: `, data);
                //after placing a bid, change user's balance permanently
                const editUser = {
                  email: this.user.email,
                  username: this.user.username,
                  balance: this.user_info.balance,
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
                    this.$buefy.toast.open({
                      message: `Bid placed! You bid ${this.amount} coins, on the ${this.pick} team.`,
                      type: "is-success",
                      duration: 4000,
                    });
                    this.amount = null;
                    this.pick = "";
                    console.log(data);
                    console.log("updated user:", this.user_info);
                  });
              });
          });
      }
    },
  },
};
</script>
<style>
.main {
  height: 100vh;
}
#no-games {
  color: #812286;
}
#no-games span {
  color: #7bc473;
}
.event-container {
  margin: 2em;
}
.game-card {
  background-color: white;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding: 0.5em;
  border-radius: 2em;
  margin: 1em;
  box-shadow: 0 4px 7px #b1b1b1;
  box-sizing: border-box;
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
#in-progress {
  color: #7bc473;
}
#final {
  color: #d997c3;
}
</style>
