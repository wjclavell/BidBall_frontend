<template>
  <div class="header">
    <b-navbar style="border-bottom: .25em solid #812286">
      <template slot="brand">
        <b-navbar-item tag="router-link" to="/">
          <img src="../assets/bidball_purpleemblem.png" id="bblogo" alt="Bid Ball emblem logo" />
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
            <button v-if="!loggedIn" class="button is-light" style="background: none">
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
            <button v-if="!loggedIn" class="button is-primary" style="background-color: #7bc473">
              <router-link style="color: white; background-color: transparent" to="/signup">
                <strong id="join">Join</strong>
              </router-link>
            </button>
          </div>
        </b-navbar-item>
        <b-navbar-item tag="router-link" to="/">
          <router-link to="/profile">
            <img
              v-if="loggedIn"
              src="https://res.cloudinary.com/wjclavell/image/upload/v1600292929/P4/bidball_default-user_lrcu7s.png"
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
  props: ["URL", "loggedIn"],
  methods: {
    logout: function () {
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
#user-profile-pic {
  width: 30px;
  height: 30px;
  border: 2px solid #278338;
  border-radius: 50%;
  transform: scale(1.5);
  margin-right: 1.5em;
}
#user-profile-pic:hover {
  cursor: pointer;
  border-color: #812286;
}
</style>
