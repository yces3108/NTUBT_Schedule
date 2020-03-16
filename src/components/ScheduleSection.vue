<template>
  <div>
    <div class="section-container">
      <h4>{{ sectionDescription }}</h4>
      <div class="section" v-for="day in days" :key="day" :day="day">
        <div 
          class="{player-section-button: true, selected: isSelected(day, section)}"
          @click="toggle(player)"
          v-for="player in trainingTable[`${day},${section}`]"
          :key="player">
          <div class="wrapper">
            <p>{{ player }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//import axios from "axios";
export default {
  name: "ScheduleSection",
  props: {
    section: Number,
    sectionDescription: String,
    trainingTable: Object,
    playerSections: Object
  },
  computed: {
  },
  data: function () {
    return {
      days: [...Array(7).keys()]
    };
  },
  methods: {
    toggle(player) {
      this.$emit('updatePlayerSections', player);
    }
  },
  mounted: function() {
  }
};
</script>

<style>
.section-container {
  display: grid;
  grid-template: auto / repeat(8, 1fr);
}
.section {
  display: inline-block;
  border: 0.5px solid rgb(39, 60, 86);
}
.player-section-button {
  display: inline-block;
  height: 20px;
  width: 20px;
  margin: 2px 2px;
  border: 0.5px solid rgb(39, 60, 86);
  text-align: center;
}
.player-section-button .selected{
  background-color: rgb(39, 60, 86);
}
.player-section-button:before {
  content: '';
  height: 100%;
  display: inline-block;
  vertical-align: middle;
}
.player-section-button .wrapper {
  display: inline-block;
  vertical-align: middle;
}

</style>