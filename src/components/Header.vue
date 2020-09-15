<template>
  <div class="header">
    <b-navbar style="border-bottom: .25em solid #812286">
      <template slot="brand">
        <b-navbar-item tag="router-link" to="/">
          <img src="../assets/bidball_greenemblem.png" id="bblogo" alt="Bid Ball emblem logo" />
        </b-navbar-item>
      </template>
      <template slot="start">
        <b-navbar-item href="#">
          <router-link to="/">Home</router-link>
        </b-navbar-item>
        <b-navbar-item href="#">
          <router-link to="/profile">Profile</router-link>
        </b-navbar-item>
        <!-- <b-navbar-dropdown label="Info">
          <b-navbar-item href="#">About</b-navbar-item>
          <b-navbar-item href="#">Contact</b-navbar-item>
        </b-navbar-dropdown>-->
      </template>
      <template slot="end">
        <b-navbar-item tag="div">
          <div class="buttons">
            <a class="button is-light" style="background: none">
              <router-link id="login" to="/login">Log in</router-link>
            </a>
            <a class="button is-primary" style="background-color: #7bc473">
              <router-link style="color: white; background-color: transparent" to="/signup">
                <strong id="join">Join</strong>
              </router-link>
            </a>
          </div>
        </b-navbar-item>
      </template>
    </b-navbar>
  </div>
</template>

<script>
export default {
  name: "Header",
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
#login:hover {
  color: #7bc473;
}
</style>
