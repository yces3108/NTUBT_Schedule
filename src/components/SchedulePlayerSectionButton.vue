<template>
  <div
    class="player-section-button cursor-pointer"
    @click="toggle"
  >
    <p 
      :class="{
        selectable: isSelectable,
        drill: isInselectedDrills,
        weight: isInselectedWeights, 
        unselectable: !isSelectable && (!isInselectedDrills && !isInselectedWeights)
        }">
      {{ player }}
    </p>
  </div>
</template>

<script>
//import axios from "axios";
export default {
  name: "SchedulePlayerSectionButton",
  props: {
    player: String,
    day: Number,
    section: Number,
    category: Number,
    selectedDrills: Array,
    selectedWeights: Array,
    numberDrills: Number,
    numberWeights: Number
  },
  computed: {
    isInselectedDrills() {
      this.numberDrills; // 因為computed不聽array，所以技巧性重新調動
      for (let section of this.selectedDrills) {
        if (JSON.stringify(section) == JSON.stringify([this.day, this.section])) {
          return true;
        }
      }
      return false;
    },
    isInselectedWeights() {
      this.numberWeights; // 因為computed不聽array，所以技巧性重新調動
      for (let section of this.selectedWeights) {
        if (JSON.stringify(section) == JSON.stringify([this.day, this.section])) {
          return true;
        }
      }
      return false;
    },
    isSelectable() {
      if (this.category == 1) {
        if (this.numberDrills < 2) {
          return true;
        } else {
          return false;
        }
      } else {
        if (this.numberWeights < 1) {
          return true;
        } else {
          return false;
        }
      }
    }
  },
  data: function () {
    return {
      days: [...Array(7).keys()],
      isSelected: false
    };
  },
  methods: {
    toggle() {
      if (
        (this.category == 1 && this.numberDrills < 2) 
        || (this.category == 2 && this.numberWeights < 1) 
        || (this.isInselectedDrills || this.isInselectedWeights)) {
        this.$emit('updateSections', [this.player, this.day, this.section, this.category]);
      }
    }
  },
  mounted: function() {
  }
};
</script>

<style>
.player-section-button {
  display: inline-block;
  height: 20px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  text-align: center;
  vertical-align: middle;
}
.player-section-button .selectable{
  border: 0.5px solid rgb(39, 60, 86);
}
.player-section-button .drill{
  border: 0.5px solid rgb(39, 60, 86);
  background-color: rgb(39, 60, 86);
  color: white;
}
.player-section-button .weight{
  border: 0.5px solid rgb(206, 49, 49);
  background-color: rgb(206, 49, 49);
  color: white;
}
.player-section-button .unselectable{
  border: 0.5px solid rgb(187, 218, 255);
  background-color: white;
  color: rgb(187, 218, 255);
  cursor: context-menu;
}

</style>