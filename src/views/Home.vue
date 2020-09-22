<template>
  <div class="home">
    <div class="banner">
      <img
        id="wordmark"
        alt="bidball wordmark"
        src="../assets/bidball_coin_wordmark.svg"
      />
      <br />
      <div class="landing-info">
        <div>
          <ul>
            <li>Create an account.</li>
            <li>Set up your profile.</li>
            <li>Bid on live games!</li>
          </ul>
        </div>
        <div>
          <router-link id="button-text" to="/signup">
            <button id="big-signup">
              Start bidding!
            </button></router-link
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  components: {},
};
</script>
<style>
.home {
  height: 100vh;
}
#wordmark {
  margin-top: 3em;
  transform: scale(0.4);
  animation: drop 3s ease-in-out;
}
.banner {
  background-image: url("https://res.cloudinary.com/wjclavell/image/upload/v1600733184/P4/purple-wallpaper_qsw3gj.png");
  background-position: center;
  background-size: cover;
  height: 85vh;
}
.landing-info {
  width: 80vw;
  margin: 0 auto;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-wrap: wrap;
}
.landing-info li {
  color: white;
  font-size: 2.5em;
  font-weight: bold;
  text-align: left;
}
#button-text {
  color: white;
}
#button-text:hover {
  color: #812286;
}
#big-signup {
  margin: 2em;
  color: white;
  position: relative;
  font-weight: bold;
  font-size: 1.5em;
  background-color: transparent;
  padding: 1em;
  border-radius: 60px;
  border: 3px solid white;
  transition: 0.08s ease-in;
  z-index: 1;
  overflow: hidden;
  background: none;
}
#big-signup:before {
  content: "";
  position: absolute;
  background: white;
  bottom: 0;
  left: 0;
  right: 0;
  top: 100%;
  z-index: -1;
  transition: top 0.14s ease-in;
}
#big-signup:hover:before {
  top: 0;
}
#big-signup:hover {
  color: #812286;
}

@keyframes drop {
  30% {
    top: -100%;
  }
  55% {
    top: 40%;
  }
  85% {
    top: -10%;
  }
  100% {
    top: 0;
  }
}

@media only screen and (max-width: 450px) {
  .landing-info li {
    font-size: 1.75em;
  }
  #big-signup {
    font-size: 1em;
  }
}
</style>
