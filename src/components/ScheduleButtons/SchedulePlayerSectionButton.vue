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
        unselectable: (!isInselectedDrills && !isInselectedWeights) && !isSelectable,
        highlighted: isHighlighted,
        pitcher: isPitcher && !isHighlighted,
        catcher: isCatcher && !isHighlighted,
        infielder: isInfielder && !isHighlighted,
        outfielder: isOutfielder && !isHighlighted,
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
    positions: Array,
    day: Number,
    section: Number,
    category: Number,
    selectedDrills: Array,
    selectedWeights: Array,
    numberDrills: Number,
    numberWeights: Number,
    highlightedPlayer: String,
    highlightedPosition: Number
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
        return (this.numberDrills < 2) ? true : false
      } else {
        return (this.numberWeights < 1) ? true : false
      }
    },
    isHighlighted() {
      return this.highlightedPlayer == this.player;
    },
    isPitcher() {
      return this.highlightedPosition == 1 && this.positions.includes("pitcher");
    },
    isCatcher() {
      return this.highlightedPosition == 2 && this.positions.includes("catcher");
    },
    isInfielder() {
      return this.highlightedPosition == 3 && this.positions.includes("infielder");
    },
    isOutfielder() {
      return this.highlightedPosition == 4 && this.positions.includes("outfielder");
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
  box-sizing: border-box;
  height: 20px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  text-align: center;
  vertical-align: middle;
}
.player-section-button .selectable {
  border: 1px solid rgb(39, 60, 86);
}
.player-section-button .drill {
  border: 1px solid rgb(39, 60, 86);
  background-color: rgb(39, 60, 86);
  color: white;
}
.player-section-button .weight {
  border: 1px solid rgb(206, 49, 49);
  background-color: rgb(206, 49, 49);
  color: white;
}
.player-section-button .unselectable {
  border: 1px solid rgb(187, 218, 255);
  background-color: white;
  color: rgb(187, 218, 255);
  cursor: context-menu;
}
.player-section-button .highlighted {
  border: 1px solid rgb(255, 197, 37);
  background-color: rgb(255, 197, 37);
  color: white;
}
.player-section-button .pitcher {
  border: 1px solid rgb(255, 112, 167);
  background-color: rgb(255, 112, 167);
  color: white;
}
.player-section-button .catcher {
  border: 1px solid rgb(0, 99, 228);
  background-color: rgb(0, 99, 228);
  color: white;
}
.player-section-button .infielder {
  border: 1px solid rgb(255, 239, 93);
  background-color: rgb(255, 239, 93);
  color: rgb(39, 60, 86);
}
.player-section-button .outfielder {
  border: 1px solid rgb(51, 156, 2);
  background-color: rgb(51, 156, 2);
  color: white;
}
</style>