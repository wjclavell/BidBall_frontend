<template>
  <div>
    <div class="categories">
      <div class="sport-wrap">
        <h1 class="labels">
          Baseball
        </h1>
        <b-icon
          @click="category"
          class="ball"
          id="3"
          pack="fas"
          icon="baseball-ball"
          size="is-large"
        ></b-icon>
      </div>
      <div class="sport-wrap">
        <h1 class="labels">
          Basketball
        </h1>
        <b-icon
          @click="category"
          class="ball"
          id="4"
          pack="fas"
          icon="basketball-ball"
          size="is-large"
        ></b-icon>
      </div>
      <div class="sport-wrap">
        <h1 class="labels">
          Football
        </h1>
        <b-icon
          @click="category"
          class="ball"
          id="2"
          pack="fas"
          icon="football-ball"
          size="is-large"
        ></b-icon>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Category",
  data: function() {
    return {
      id: 2,
    };
  },
  methods: {
    category: function() {
      // this.id = event.target.id;
      // console.log(this.id);
      // this.$emit(this.id);
      console.log("method is activated");
    },
  },
};
</script>

<style>
.categories {
  display: flex;
  justify-content: space-around;
  align-items: center;
  border-top: 2px solid #b1b1b1;
  border-bottom: 2px solid #b1b1b1;
  margin-bottom: 3em;
}
.sport-wrap {
  display: flex;
  align-items: center;
  padding: 0.5em;
}
.labels {
  font-size: 2em;
  margin-right: 1em;
}
.ball {
  color: #b1b1b1;
}
.ball:hover {
  color: #50b963;
  cursor: pointer;
}
</style>
