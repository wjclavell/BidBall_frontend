<template>
  <div class="home">
    <img alt="bidball wordmark" src="../assets/bidball_wordmark.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <br />
    <h1>
      <strong>This will be the landing page</strong>
    </h1>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
  },
};
</script>
<style>
.home {
  height: 100vh;
}
</style>
