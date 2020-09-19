<template>
  <div class="profile">
    <div class="user-left">
      <h1 id="profile-header">{{ user_info.username }}'s Profile</h1>
      <img
        :src="user_info.profile_pic"
        id="profile-pic"
        alt="profile picture"
      />
      <div class="record">
        <h1>{{ user_info.correct }} - {{ user_info.incorrect }}</h1>
      </div>
    </div>
    <div class="user-mid">
      <h1>Favorite Sport: {{ user_info.favorite_league }}</h1>
      <h1>Favorite Teams:</h1>
      <div class="team-container">
        <p>New York Yankees</p>
        <p>Miami Heat</p>
        <p>Los Angeles Lakers</p>
        <p>New York Giants</p>
      </div>
    </div>
    <Collapse />
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
#profile-pic {
  border: 0.5em solid #50b963;
  border-radius: 100%;
  height: 15em;
  width: 15em;
}
</style>
