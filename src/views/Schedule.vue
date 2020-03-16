<template>
  <div class="schedule">
    <div>
      <div :class="{
        drillActive: trainingCategory==1,
        drillNonactive: trainingCategory!=1,
        trainingButton: true
      }"
        @click="changeTrainingToDrill"
      >
        小練
      </div>
      <div :class="{
        weightActive: trainingCategory==2,
        weightNonactive: trainingCategory!=2,
        trainingButton: true
      }"
        @click="changeTrainingToWeight"
      >
        重訓
      </div>
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
          ><!-- strangeCount是要讓vue實例知道要重新渲染 -->
            <SchedulePlayerSectionButton              
              v-for="player in trainingTable[`${day},${sectionIndex}`]"
              :key="player"
              :player="player"
              :day="day"
              :section="sectionIndex"
              :category="trainingCategory"
              :selectedDrills="playerSections[1][player]"
              :numberDrills="playerSections[1][player].length"
              :selectedWeights="playerSections[2][player]"
              :numberWeights="playerSections[2][player].length"
              @updateSections="updatePlayerSections"
            />
          </div>
        </div>
      </div>
    </div>

    <div 
      class="player-count-section"
      v-for="(position, player) of playerProfile" 
      :key="player"
      :player="player"
      :position="position"
      :strangeCount="strangeCount"
    ><!-- strangeCount是要讓vue實例知道要重新渲染 -->
      <div 
        class="player-schedule-button">
        {{ player }}
      </div>
      <div 
        v-for="(sectionDrill, indexDrill) of playerSections[1][player]"
        :key="indexDrill"
        class="drill-count">
      </div>
      <div 
        v-for="(sectionWeight, indexWeight) of playerSections[2][player]"
        :key="indexWeight"
        class="weight-count">
      </div>
      <p class="wrapper">
      </p>
    </div>
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
import ScheduleSectionButton from "@/components/ScheduleSectionButton.vue";
import SchedulePlayerSectionButton from "@/components/SchedulePlayerSectionButton.vue";
//import SchedulePlayer from "@/components/SchedulePlayer.vue";
// @ is an alias to /src

export default {
  name: "Home",
  components: {
    ScheduleSectionButton,
    SchedulePlayerSectionButton
    //SchedulePlayer
  },
  data: function () {
    return {
      trainingCategory: 1, // 1:小練, 2:重訓
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
      strangeCount: 0
    };
  },
  computed: {
  },
  methods: {
    changeTrainingToDrill() {
      this.trainingCategory = 1;
    },
    changeTrainingToWeight() {
      this.trainingCategory = 2;
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
.trainingButton {
  cursor: pointer;
  display: inline-block;
  list-style-type: none;
  width: 100px;
  margin: 5px 10px;
  border-radius: 5px; 
  padding: 5px 10px;
  font-size:  1.2rem;
}
.drillNonactive {
  background-color: white;
  color: rgb(39, 60, 86);  
  border: 3px solid rgb(39, 60, 86); 
}
.drillNonactive:hover {
  background-color: rgb(39, 60, 86);
  color: white;  
  border: 3px solid rgb(39, 60, 86)
}
.drillActive {
  background-color: rgb(39, 60, 86);
  color: white;  
  border: 3px solid rgb(39, 60, 86)
} 
.weightNonactive {
  background-color: white;
  color: rgb(206, 49, 49);
  border: 3px solid rgb(206, 49, 49);
}
.weightNonactive:hover {
  background-color: rgb(206, 49, 49);
  color: white;  
  border: 3px solid rgb(206, 49, 49);
}
.weightActive {
  background-color: rgb(206, 49, 49);
  color: white;  
  border: 3px solid rgb(206, 49, 49);
}
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
.player-count-section {
  display: inline-block;
}
.player-schedule-button {
  display: block;
  height: 20px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  border: 0.5px solid rgb(39, 60, 86);
  text-align: center;
  vertical-align: middle;
}
.drill-count {
  display: block;
  height: 5px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  border: 0.5px solid rgb(39, 60, 86);
  background-color: rgb(39, 60, 86);
  text-align: center;
  vertical-align: middle;
}
.weight-count {
  display: block;
  height: 5px;
  line-height: 20px;
  width: 20px;
  margin: 2px 2px;
  border: 0.5px solid rgb(206, 49, 49);
  background-color: rgb(206, 49, 49);
  text-align: center;
  vertical-align: middle;
}
</style>
