<template>
  <div>
    <div
      class="player-count-section"
      v-for="(position, player) of playerProfile"
      :key="player"
      :player="player"
      :position="position"
      :strangeCount="strangeCount"
    >
      <!-- strangeCount是要讓vue實例知道要重新渲染 -->
      <div class="player-schedule-button" @click="highlight(player)">
        <div
          :class="{
            highlighted: player == highlightedPlayer,
            unhighlighted: player != highlightedPlayer,
            pitcher: isPitcher(player) && player != highlightedPlayer,
            catcher: isCatcher(player) && player != highlightedPlayer,
            infielder: isInfielder(player) && player != highlightedPlayer,
            outfielder: isOutfielder(player) && player != highlightedPlayer
          }"
        >
          {{ player }}
        </div>
      </div>
      <div
        v-for="(sectionDrill, indexDrill) of playerDrillSections[player]"
        :key="indexDrill"
        :strangeCount="strangeCount"
        class="drill-count"
      />
      <div
        v-for="(sectionWeight, indexWeight) of playerWeightSections[player]"
        :key="indexWeight"
        :strangeCount="strangeCount"
        class="weight-count"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "PlayerCount",
  props: {
    playerProfile: Object,
    playerDrillSections: Object,
    playerWeightSections: Object,
    highlightedPlayer: String,
    highlightedPosition: Number,
    strangeCount: Number
  },
  methods: {
    highlight(player) {
      this.$emit("highlightPlayer", player);
    },
    isPitcher(player) {
      return this.highlightedPosition == 1 && this.playerProfile[player]['position'].includes("pitcher");
    },
    isCatcher(player) {
      return this.highlightedPosition == 2 && this.playerProfile[player]['position'].includes("catcher");
    },
    isInfielder(player) {
      return this.highlightedPosition == 3 && this.playerProfile[player]['position'].includes("infielder");
    },
    isOutfielder(player) {
      return this.highlightedPosition == 4 && this.playerProfile[player]['position'].includes("outfielder");
    }
  }
};
</script>

<style>

.player-schedule-button .highlighted {
  background-color: rgb(255, 197, 37);
  color: white;
  border: 0.5px solid rgb(255, 197, 37);
}
.player-schedule-button .unhighlighted {
  border: 0.5px solid rgb(39, 60, 86);
}
.player-schedule-button .pitcher {
  border: 1px solid rgb(255, 75, 144);
  background-color: rgb(255, 75, 144);
  color: white;
}
.player-schedule-button .catcher {
  border: 1px solid rgb(0, 99, 228);
  background-color: rgb(0, 99, 228);
  color: white;
}
.player-schedule-button .infielder {
  border: 1px solid rgb(255, 239, 93);
  background-color: rgb(255, 239, 93);
  color: rgb(39, 60, 86);
}
.player-schedule-button .outfielder {
  border: 1px solid rgb(51, 156, 2);
  background-color: rgb(51, 156, 2);
  color: white;
}
.player-count-section {
  display: inline-block;
}
.player-schedule-button {
  cursor: pointer;
  display: block;
  height: 20px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  text-align: center;
}
.drill-count {
  display: block;
  box-sizing: border-box;
  height: 5px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  border: 0.5px solid rgb(39, 60, 86);
  background-color: rgb(39, 60, 86);
  text-align: center;
}
.weight-count {
  display: block;
  box-sizing: border-box;
  height: 5px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  border: 0.5px solid rgb(206, 49, 49);
  background-color: rgb(206, 49, 49);
  text-align: center;
}
</style>
