<template>
  <div id="calendar">
    <div id="current-day-info" class="color">
      <h1 id="app-name-landscape" class="off-color default-cursor center">
        My Calendar
      </h1>
      <div>
        <h2 id="cur-date" class="current-day-heading dfault-cursor center">
          {{ today }}
        </h2>
        <h2 id="cur-year" class="current-day-heading dfault-cursor center">
          {{ getYear }}
        </h2>
        <h2 id="cur-day" class="current-day-heading default-cursor center">
          {{ getWeekDay }}
        </h2>
        <h2 id="cur-month" class="current-day-heading default-cursor center">
          {{ getMonth }}
        </h2>
        <h2 id="cur-date" class="current-day-heading default-cursor center">
          {{ getDate }}
        </h2>
      </div>
      <button class="font button">Change theme</button>
    </div>
    <h3 id="cal-year">{{ getCalendarYear() }}</h3>
    <div>
      <font-awesome-icon class="cursor-pointer cal-button" :icon="['fas', 'caret-left']" @click="changeToPreviousMonth" />
      <h3 id="cal-month" class="cursor-pointer cal-button" @click="changeToThisMonth">{{ getCalendarMonth() }}</h3>
      <font-awesome-icon class="cursor-pointer cal-button" :icon="['fas', 'caret-right']" @click="changeToNextMonth" />
    </div>
    <div class="weekday-container">
      <h5 class="weekday border-color">Sun</h5>
      <h5 class="weekday border-color">Mon</h5>
      <h5 class="weekday border-color">Tue</h5>
      <h5 class="weekday border-color">Wed</h5>
      <h5 class="weekday border-color">Thu</h5>
      <h5 class="weekday border-color">Fri</h5>
      <h5 class="weekday border-color">Sat</h5>
    </div>
    <div class="week-container">
      <CalendarWeek 
        :calendarYear ="calendarYear"
        :calendarMonth ="calendarMonth"
        :expandedWeek="expandedWeek"
        @changeExpandedWeek="updateExpandedWeek"
        :weekIndex="weekIndex"
        :key="weekIndex"  
        v-for="weekIndex in [0,1,2,3,4,5]"  
      />
    </div>
  </div>
</template>

<script src="https://kit.fontawesome.com/e098e2022e.js" crossorigin="anonymous"></script>
<script>
import CalendarWeek from "@/components/CalendarWeek.vue";
// @ is an alias to /src
export default {
  name: "Calendar",
	components: {
    CalendarWeek
  },
  data: function () {
    return {
      today: new Date(),
      weekDayNames: ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"],
      monthNames: ["January","February","March","April","May","June","July","August","September","October","November","December"],
      calendarYear: Number,
      calendarMonth: Number,
      weeks: [0,1,2,3,4,5],
      expandedWeek: -1 // 都還沒展開
    }
  },
  props: {

  },
  computed: {
    getYear() {
      return this.today.getFullYear();
    },
    getWeekDay() {
      return this.weekDayNames[this.today.getDay()];
    },
    getMonth() {
      return this.monthNames[this.today.getMonth()];
    },
    getDate() {
      return this.today.getDate();
    }
  },
  beforeMount: function() {
    this.calendarYear = this.today.getFullYear();
    this.calendarMonth = this.today.getMonth();
  },
  methods: {
    getCalendarYear() {
      return this.calendarYear;
    },
    getCalendarMonth() {
      return this.monthNames[this.calendarMonth];
    },
    changeToPreviousMonth() {
      this.calendarMonth--;
      if( this.calendarMonth == -1) {
        this.calendarYear--;
        this.calendarMonth = 11;
      }
      this.expandedWeek = -1;
    },
    changeToThisMonth() {
      let today = new Date();
      this.calendarYear = today.getFullYear();
      this.calendarMonth = today.getMonth();
      this.expandedWeek = -1;
    },
    changeToNextMonth() {
      this.calendarMonth++;  
      if( this.calendarMonth == 12) {
        this.calendarYear++;
        this.calendarMonth = 0;
      }
      this.expandedWeek = -1;
    },
    updateExpandedWeek(changedWeek) {
      if (changedWeek == this.expandedWeek) {
        this.expandedWeek = -1 // 回到初始狀態
      } else {
        this.expandedWeek = changedWeek
      }
    }
  }
};
</script>

<style>
.cal-button {
  display: inline-block;
  vertical-align:middle;
  width: 100px;
  padding: 10px 10px;
}
.weekday-container {
    display: grid;
    grid-template: auto / repeat(7, 1fr)
}
.weekday {
    font-size: 16px;
    font-weight: 300;
    padding: 10px;
    margin: 5px;
}
.week-container {
  height: 300px;
  display: grid;
  grid-template: repeat(6, 1fr) / auto;
}
</style>