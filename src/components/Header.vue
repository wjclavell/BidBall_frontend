<template>
  <div class="header">
    <b-navbar style="border-bottom: .25em solid #812286">
      <template slot="brand">
        <b-navbar-item v-if="!loggedIn" tag="router-link" to="/">
          <img
            src="../assets/bidball_purpleemblem.png"
            id="bblogo"
            alt="Bid Ball emblem logo"
          />
        </b-navbar-item>
        <b-navbar-item v-if="loggedIn" tag="router-link" to="/main">
          <img
            src="../assets/bidball_purpleemblem.png"
            id="bblogo"
            alt="Bid Ball emblem logo"
          />
        </b-navbar-item>
      </template>
      <template slot="start">
        <b-navbar-item href="#">
          <router-link v-if="loggedIn" to="/main">Home</router-link>
        </b-navbar-item>
        <b-navbar-item href="#">
          <router-link v-if="loggedIn" to="/profile">Profile</router-link>
        </b-navbar-item>
        <!-- <b-navbar-dropdown label="Info">
          <b-navbar-item href="#">About</b-navbar-item>
          <b-navbar-item href="#">Contact</b-navbar-item>
        </b-navbar-dropdown>-->
      </template>
      <template slot="end">
        <b-navbar-item tag="div">
          <div class="buttons">
            <button
              v-if="!loggedIn"
              class="button is-light"
              style="background: none"
            >
              <router-link id="login" to="/login">Log in</router-link>
            </button>
            <button
              @click="logout"
              v-if="loggedIn"
              class="button is-light"
              style="background: none"
            >
              <router-link id="logout" to="/">Log out</router-link>
            </button>
            <button
              v-if="!loggedIn"
              class="button is-primary"
              style="background-color: #7bc473"
            >
              <router-link
                style="color: white; background-color: transparent"
                to="/signup"
              >
                <strong id="join">Join</strong>
              </router-link>
            </button>
          </div>
        </b-navbar-item>
        <b-navbar-item>
          <div id="balance">
            <p v-if="loggedIn">{{ user_info.balance }}</p>
            <img
              v-if="loggedIn"
              src="../assets/Gamebits.svg"
              id="gamebits"
              alt="default user"
            />
          </div>
        </b-navbar-item>
        <b-navbar-item>
          <router-link to="/profile">
            <img
              v-if="loggedIn"
              :src="user.profile_pic"
              id="user-profile-pic"
              alt="default user"
            />
          </router-link>
        </b-navbar-item>
      </template>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: "Header",
  props: ["url", "loggedIn", "user"],
  data: function() {
    return {
      URL: this.url,
      user_info: this.user,
    };
  },
  methods: {
    logout: function() {
      this.$emit("logout");
    },
  },
};
</script>

<style>
.header {
  color: white;
}
#bblogo {
  margin-right: 0.5em;
  margin-left: 2em;
  margin-bottom: 1em;
  transform: scale(2);
}
.burger {
  margin-right: 1.5em;
  transform: scale(2);
}
.burger span {
  transform: scale(1.1);
  font-weight: bold;
}
#join:hover {
  color: #812286;
}
#login:hover,
#logout:hover {
  color: #7bc473;
}
#balance p {
  color: #50b963;
}
#user-profile-pic {
  width: 28px;
  height: 30px;
  /* border: 2px solid #50b963; */
  border-radius: 15px;
  transform: scale(1.5);
  margin-right: 1.5em;
}
#user-profile-pic:hover {
  cursor: pointer;
  filter: grayscale(0.5);
}
</style>
