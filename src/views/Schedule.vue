<template>
  <div class="schedule">
    <div>
      <TrainingSwitch @toggleTraining="changeTraining" />
      <PositionSwitch
        :highlightedPosition="highlightedPosition"
        @changePosition="changePositionTo"
        @changePositionToPithcer="changePositionTo"
        @changePositionToCatcher="changePositionTo"
        @changePositionToInfielder="changePositionTo"
        @changePositionToOutfielder="changePositionTo"
      />
    </div>
    <div class="schedule weekday-container">
      <h5 class="weekday border-color">時間</h5>
      <h5 class="weekday border-color">日</h5>
      <h5 class="weekday border-color">一</h5>
      <h5 class="weekday border-color">二</h5>
      <h5 class="weekday border-color">三</h5>
      <h5 class="weekday border-color">四</h5>
      <h5 class="weekday border-color">五</h5>
      <h5 class="weekday border-color">六</h5>
    </div>
    <div class="day-section-container">
      <div
        class="section-container"
        v-for="(sectionDescription, sectionIndex) in sectionDescriptions"
        :key="sectionIndex"
      >
        <ScheduleSectionButton
          :index="sectionIndex"
          :description="sectionDescription"
          @toggleThisSection="toggleSection"
        />
        <div class="section" v-for="day in days" :key="day" :day="day">
          <div
            v-if="!isFoldedSections[sectionIndex]"
            :strangeCount="strangeCount"
          >
            <!-- strangeCount是要讓vue實例知道要重新渲染 -->
            <SchedulePlayerSectionButton
              v-for="player in trainingTable[`${day},${sectionIndex}`]"
              :key="player"
              :player="player"
              :positions="playerProfile[player]['position']"
              :day="day"
              :section="sectionIndex"
              :category="trainingCategory"
              :selectedDrills="playerSections[1][player]"
              :numberDrills="playerSections[1][player].length"
              :selectedWeights="playerSections[2][player]"
              :numberWeights="playerSections[2][player].length"
              :highlightedPlayer="highlightedPlayer"
              :highlightedPosition="highlightedPosition"
              @updateSections="updatePlayerSections"
            />
          </div>
        </div>
      </div>
    </div>
    <PlayerCount
      :playerProfile="playerProfile"
      :playerSections="playerSections"
      :playerDrillSections="playerSections[1]"
      :playerWeightSections="playerSections[2]"
      :highlightedPlayer="highlightedPlayer"
      :highlightedPosition="highlightedPosition"
      :strangeCount="strangeCount"
      @highlightPlayer="highlight"
    />
<!--
    <SchedulePlayer 
      v-for="(position, player) of playerProfile" 
      :key="player"
      :player="player"
      :position="position"
      :sections="playerSections[player]"
      :strangeCount="strangeCount"
    />
    -->
  </div>
</template>

<script>
import trainingTimeJSON from "../assets/trainingTime.json"
import playerProfileJSON from "../assets/playerProfile.json"
import ScheduleSectionButton from "@/components/ScheduleButtons/ScheduleSectionButton.vue";
import SchedulePlayerSectionButton from "@/components/ScheduleButtons/SchedulePlayerSectionButton.vue";
import TrainingSwitch from "@/components/ScheduleButtons/TrainingSwitch.vue";
import PositionSwitch from "@/components/ScheduleButtons/PositionSwitch.vue";
import PlayerCount from "@/components/ScheduleButtons/PlayerCount.vue";
//import SchedulePlayer from "@/components/SchedulePlayer.vue";
// @ is an alias to /src

export default {
  name: "Schedule",
  components: {
    ScheduleSectionButton,
    SchedulePlayerSectionButton,
    TrainingSwitch,
    PositionSwitch,
    PlayerCount
    //SchedulePlayer
  },
  data: function () {
    return {
      trainingCategory: 2, // 1:小練, 2:重訓
      sections: [...Array(7).keys()],
      sectionDescriptions: [
        "7:00~9:00",
        "8:10~10:00",
        "10:20~12:00",
        "12:20~14:10",
        "13:20~15:10",
        "15:30~17:20",
        "晚間"],
      days: [...Array(7).keys()],
      trainingTime: JSON,
      trainingTable: Object,
      isFoldedSections: [false, false, false, false, false, false, false],
      playerSections: Object,
      playerProfile: Object,
      highlightedPlayer: "",
      highlightedPosition: 0,
      strangeCount: 0
      
    };
  },
  computed: {
  },
  methods: {
    changeTraining(training) {
      (training == 1) ? this.trainingCategory = 1 : this.trainingCategory = 2;
    },
    changePositionTo(position) {
      if (position == this.highlightedPosition) {
        this.highlightedPosition = 0;
      } else {
        this.highlightedPosition = position;
      }
    },
    getTrainingTable(trainingTime) {
      let trainingTable = new Object;
      for (let player in trainingTime) {
        for (let section of trainingTime[player]) {
          if (!Object.keys(trainingTable).includes(String(section))) {
            trainingTable[section] = new Array();
          }
          trainingTable[section].push(player);
        }
      }
      return trainingTable
    },
    toggleSection(section) {
      this.isFoldedSections[section] = !this.isFoldedSections[section];
      this.strangeCount++;
    },
    updatePlayerSections([player, day, section, category]) {
      let indexOfDuplicateSection = [-1, -1];
      for (let i of [1, 2]) {
        for (let j in this.playerSections[i][player]) {
          if (JSON.stringify(this.playerSections[i][player][j]) == JSON.stringify([day, section])) {
            indexOfDuplicateSection = [i, j];
          }
        }
      }
      if (indexOfDuplicateSection[0] != -1) {
        this.playerSections[indexOfDuplicateSection[0]][player].splice(indexOfDuplicateSection[1], 1);
      } else {
        this.playerSections[category][player].push([day, section]);
      }
      this.strangeCount++;
    },
    highlight(player) {
      if (this.highlightedPlayer == player) {
        this.highlightedPlayer = "";
      } else {
        this.highlightedPlayer = player;
      }
    }
  },
  created() {
    this.trainingTime = trainingTimeJSON;
    this.trainingTable = this.getTrainingTable(this.trainingTime);
    this.playerProfile = playerProfileJSON;
    this.playerSections[1] = new Object();
    this.playerSections[2] = new Object();
    for (let player in this.trainingTime) {
      this.playerSections[1][player] = [];
      this.playerSections[2][player] = [];
    }
  }
};
</script>

<style>
.schedule .weekday-container {
  display: grid;
  grid-template: auto / repeat(8, 1fr);
}
.weekday {
  font-size: 16px;
  font-weight: 300;
  padding: 10px;
  margin: 5px;
  text-align: center;
}
.day-section-container {
  display: grid;
  grid-template: repeat(7, auto) / auto;
}
.section-container {
  display: grid;
  grid-template: auto / repeat(8, 1fr);
}
.section {
  border: 0.5px solid rgb(39, 60, 86);
}
.section-description {
  line-height: 100%;
  text-align: center;
  vertical-align: middle;
}
</style>
