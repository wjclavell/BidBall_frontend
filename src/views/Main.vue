<template>
  <div class="main">
    <button class="get" @click="getEvents">Show the events!</button>
  </div>
</template>

<script>
export default {
  name: "Main",
  components: {},
  props: ["token"],
  data: function () {
    return {
      URL: "http://localhost:8000/",
    };
  },
  methods: {
    getEvents: function () {
      fetch(`${this.URL}api/games/`, {
        method: "get",
        headers: {
          "Content-Type": "application/json",
        },
      })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
          console.table(data);
        });
    },
  },
};
</script>
<style>
.main {
  height: 100vh;
}
</style>