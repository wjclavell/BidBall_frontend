<template>
  <div class="profile">
    <section class="dashboard">
      <div class="user-left">
        <h1 id="profile-header">{{ user_info.username }}</h1>
        <b-tooltip
          label="Click to change picture"
          position="is-right"
          type="is-light"
        >
          <b-collapse :open="false" aria-id="contentIdForA11y1">
            <img
              :src="user_info.profile_pic"
              id="profile-pic"
              alt="profile picture"
              slot="trigger"
              aria-controls="contentIdForA11y1"
            />
            <div class="content">
              <b-field label="Change Profile Picture" label-position="inside">
                <b-input
                  v-model="profile_pic"
                  placeholder="Image URL"
                ></b-input>
                <p class="control">
                  <button
                    @click="changePic"
                    class="button"
                    style="background-color: #7bc473; color: white"
                  >
                    Change
                  </button>
                </p>
              </b-field>
            </div>
          </b-collapse>
        </b-tooltip>
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
            <span
              ><Collapse
                :user_info="user"
                :url="URL"
                @addFav="favorites($event)"
            /></span>
          </h4>
          <div class="team-container">
            <p
              class="team-names"
              v-for="item in user_info.favorite_teams"
              :key="item"
            >
              {{ item }} |
            </p>
          </div>
        </div>
      </div>
      <div class="user-right">
        <div class="user-balance">
          <h1>Balance:</h1>
          <p>{{ user_info.balance }}</p>
          <img id="game-bit" src="../assets/bidball_greenemblem.png" />
        </div>
        <div class="bid-container">
          <h1>
            Recent Bids:
            <b-icon
              id="refresh-bids"
              pack="fas"
              icon="sync-alt"
              size="is-small"
              @click.native="getBids"
            ></b-icon>
          </h1>
          <ul>
            <li v-for="bid in userBids" :key="bid">
              {{ bid.created_at.substring(0, 10) }} |
              <strong>{{ bid.amount }}</strong> | {{ bid.team }} |
              <strong
                ><u>{{ bid.result }}</u></strong
              >
            </li>
          </ul>
        </div>
      </div>
    </section>
    <a id="delete-account" @click="confirmDelete"> delete account </a>
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
  data: function () {
    return {
      user_info: this.user,
      URL: this.url,
      profile_pic: "",
      userBids: "",
    };
  },
  bidHistory: function () {
    this.getBids();
  },
  methods: {
    //! for some reason this method only works after u change your picture...
    favorites: function (team) {
      //update user favorite_teams field
      this.user_info.favorite_teams.push(team);
      this.user_info.favorite_teams;
      const editUser = {
        email: this.user.email,
        username: this.user.username,
        favorite_teams: this.user_info.favorite_teams,
      };
      fetch(`${this.URL}auth/users/profile/`, {
        method: "PUT",
        headers: {
          "Content-Type": "application/json",
          Authorization: `JWT ${this.user.token}`,
        },
        body: JSON.stringify(editUser),
      }).then((response) => response.json());
    },
    changePic: function () {
      const editUser = {
        email: this.user.email,
        username: this.user.username,
        profile_pic: this.profile_pic,
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
          this.profile_pic = "";
          this.user_info = data;
          this.$emit("reassign", data);
        });
    },
    confirmDelete: function () {
      this.$buefy.dialog.confirm({
        title: "Deleting account",
        message:
          "Are you sure you want to <b>delete</b> your account? This action cannot be undone.",
        confirmText: "Delete Account",
        type: "is-danger",
        hasIcon: true,
        onConfirm: () => this.deleteAccount(),
      });
    },
    deleteAccount: function () {
      fetch(`${this.URL}auth/users/profile`, {
        method: "DELETE",
        headers: {
          "Content-type": "application/json",
          Authorization: `JWT ${this.user_info.token}`,
        },
      }).then((response) => {
        this.$buefy.toast.open("Account deleted!");
        this.$emit("logout", response);
      });
    },
    getBids: function () {
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
          this.userBids = data.results;
        });
    },
  },
  mounted: function () {
    this.getBids();
  },
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
  width: 250px;
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
  /* border: 0.5em solid #50b963; */
  border-radius: 100%;
  height: 12em;
  width: 12em;
}
#profile-pic:hover {
  filter: opacity(0.5);
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
  color: #9c509f;
}
.user-mid {
  width: 250px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  margin-bottom: 2em;
}
.fav-sport h4 {
  font-weight: bold;
}
.fav-sport h1 {
  font-size: 1.5em;
  font-weight: bold;
  color: #812286;
}
.fav-teams h4 {
  font-weight: bold;
}
.team-names {
  color: #50b963;
  font-weight: bold;
  display: inline;
}
.user-right {
  width: 250px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.user-balance {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
}
.user-balance h1 {
  font-weight: bold;
}
.user-balance p {
  font-size: 1.5em;
  font-weight: bold;
  color: #7bc473;
}
.bid-container {
  height: 60%;
}
#refresh-bids {
  color: #812286;
}
#refresh-bids:hover {
  cursor: pointer;
  animation: rotate 2s linear infinite;
}
#delete-account {
  color: rgb(248, 73, 73);
}

@keyframes rotate {
  to {
    transform: rotate(0deg);
  }
  from {
    transform: rotate(-360deg);
  }
}
</style>
