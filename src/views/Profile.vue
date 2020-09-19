<template>
  <div class="profile">
    <section class="dashboard">
      <div class="user-left">
        <h1 id="profile-header">{{ user_info.username }}</h1>
        <img
          :src="user_info.profile_pic"
          id="profile-pic"
          alt="profile picture"
        />
        <h1 id="record">
          <span id="win">{{ user_info.correct }}</span> -
          <span id="loss">{{ user_info.incorrect }}</span>
        </h1>
      </div>
      <div class="user-mid">
        <div class="fav-sport">
          <h4>Favorite Sport:</h4>
          <h1>{{ user_info.favorite_league }}</h1>
        </div>
        <div class="fav-teams">
          <h4>
            Favorite Teams:
            <span><Collapse /></span>
          </h4>
          <div class="team-container">
            <p>New York Yankees</p>
            <p>Miami Heat</p>
            <p>Los Angeles Lakers</p>
            <p>New York Giants</p>
          </div>
        </div>
      </div>
      <div class="user-right">
        <div class="user-balance">
          <h1>Balance:</h1>
          <p>{{ user_info.balance }}</p>
          <img id="game-bit" src="../assets/bidball_greenemblem.png" />
        </div>
        <h1>Recent Bids:</h1>
        <div class="bid-container">
          <ul>
            <li>
              2020-09-16 | <strong>30</strong> | NYY |
              <strong><u>W</u></strong>
            </li>
            <li>
              2020-09-16 | <strong>25</strong> | MIA |
              <strong><u>W</u></strong>
            </li>
            <li>
              2020-09-15 | <strong>25</strong> | BAL |
              <strong><u>L</u></strong>
            </li>
          </ul>
        </div>
      </div>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import Collapse from "../components/Collapse.vue";

export default {
  name: "Profile",
  components: {
    Collapse,
  },
  props: ["user", "url"],
  data: function() {
    return {
      user_info: this.user,
      URL: this.url,
    };
  },
  // methods: {
  //   assignInfo
  // }
};
</script>
<style>
.profile {
  min-height: 100vh;
}
.dashboard {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 2em;
  padding: 1em;
  min-height: 50vh;
  background-color: white;
  border-radius: 1em;
  box-shadow: 0 4px 7px #b1b1b1;
}
.user-left {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  margin-bottom: 2em;
}
#profile-header {
  font-size: 2em;
  font-weight: bold;
}
#profile-pic {
  border: 0.5em solid #50b963;
  border-radius: 100%;
  height: 12em;
  width: 12em;
}
#record {
  font-weight: bold;
}
#win {
  font-size: 1.5em;
  color: #7bc473;
}
#loss {
  font-size: 1.5em;
  color: #812286;
}
.user-mid {
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  margin-bottom: 2em;
}
</style>
