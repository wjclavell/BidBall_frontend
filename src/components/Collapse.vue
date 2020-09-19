<template>
  <section>
    <b-collapse :open="false" aria-id="contentIdForA11y1">
      <button
        id="add-collapse"
        class="button is-primary"
        slot="trigger"
        aria-controls="contentIdForA11y1"
        style="background-color: #812286"
      >
        <b-icon
          aria-controls="contentIdForA11y1"
          id="add"
          pack="fas"
          icon="plus"
          size="is-small"
        ></b-icon>
      </button>

      <div class="notification">
        <div class="content">
          <b-field label="Favorite Sport">
            <b-select v-model="team" placeholder="Favorite Sport">
              <option>Baseball</option>
              <option>Basketball</option>
              <option>Football</option>
            </b-select>
          </b-field>
          <button
            class="button is-primary"
            @click="addTeam"
            style="background-color: #7bc473"
          >
            Add
          </button>
        </div>
      </div>
    </b-collapse>
  </section>
</template>

<script>
// let teams = "76ers Bucks Bulls Cavaliers Celtics Clippers Grizzlies Hawks Heat Hornets Jazz Kings Knicks Lakers Magic Mavericks Nets Nuggets Pacers Pelicans Pistons Raptors Rockets Spurs Suns Thunder Timberwolves TrailBlazers Warriors Wizards"

export default {
  name: "Collapse",
  data: function() {
    return {
      team: "",
    };
  },
  methods: {
    addTeam: function() {
      this.$emit("addFav", this.team);
    },
  },
};
</script>
<style>
#add-collapse {
  transform: scale(0.5);
}
</style>
