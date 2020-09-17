<template>
  <div class="main">
    <button
      class="button is-primary"
      style="background-color: #7bc473"
      @click="dataWorking"
    >
      Show the events!
    </button>
    <!-- WILL MAKE A TEMPLATE CARD AND LOOP THROUGH EACH EVENT TO CREATE THE CARDS FOR ALL DATA FROM REQUEST -->
  </div>
</template>

<script>
const unirest = require("unirest"); // unirest library used to make requests to 'the rundown' api

export default {
  name: "Main",
  components: {},
  props: ["token"],
  data: function() {
    return {
      URL: "http://localhost:8000/",
      winner: "away",
      date: new Date() //convert today's date to yyyy-mm-dd format, used to get today's games
        .toJSON()
        .slice(0, 10)
        .replace(/-/g, "/"),
    };
  },
  methods: {
    // method to get all the daily games/events by sport
    getEvents: function() {
      //parameters after 'date' are optional
      //* /sports/{sport_id}/events/{yyyy-mm-dd}?include={periods}&include={scores}&offset={240}

      let req = unirest(
        "GET",
        "https://therundown-therundown-v1.p.rapidapi.com/sports/3/events/2020-09-16"
      );

      req.query({
        include: ["full-game", "scores"],
        offset: "240",
      });

      req.headers({
        "x-rapidapi-host": "therundown-therundown-v1.p.rapidapi.com",
        "x-rapidapi-key": "44d40d3cbfmsh8e33a571ba5cc18p1c5131jsn6c6e002bb94e",
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
          res.body.events[0].teams[0].name,
          res.body.events[0].score.score_away,
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
        "x-rapidapi-key": "44d40d3cbfmsh8e33a571ba5cc18p1c5131jsn6c6e002bb94e",
        useQueryString: true,
      });

      req.end(function(res) {
        if (res.error) throw new Error(res.error);

        console.log(res.body);
      });
    },
    dataWorking: function() {
      // checking if variables are correct
      console.log(this.winner, this.date);
    },
  },
};
</script>
<style>
.main {
  height: 100vh;
}
</style>
