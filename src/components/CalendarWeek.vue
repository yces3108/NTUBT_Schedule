<template>
  <div class="week" @click="expand">
    <CalendarDate 
      :weekMonth="calendarMonth"
      :date="date"
      :isExpanded="isExpanded"
      :key="index"
      v-for="(date, index) in getCalendarDateArray"
    />
  </div>
</template>

<script>
import CalendarDate from "@/components/CalendarDate.vue";
export default {
  name: "CalendarWeek",
  components: {
    CalendarDate
  },
  props: {
    weekIndex: Number,
    calendarYear: Number,
    calendarMonth: Number,
    expandedWeek: Number
  },
  data: function () {
    return {
    };
  },
  computed: {
    getCalendarDateArray() {
      //this.collapseWeek(); // 把展開的星期摺疊回去
      let curYear = this.calendarYear;
      let curMonth = this.calendarMonth;
      let calendarDateArray = []
      const monthDays = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]; // 第一個元素放個啞巴元素，好讓我們可以用1~12來存取月份的天數
      //判斷今年是否是閏年
      if ((curYear % 4 == 0 && curYear % 100 != 0) || curYear % 400 == 0) {
        monthDays[1] = 29;
      }
      let weekDay = new Date(curYear, curMonth, 1).getDay(); // 取得今年今月1日為禮拜幾
      let prevMonth = curMonth - 1;
      if (prevMonth == -1) prevMonth = 11;
      for (var i = this.weekIndex * 7; i < (this.weekIndex + 1) * 7; i++) {
        // 上個月
        if (i < weekDay) {
          calendarDateArray.push(new Date(curYear,
          curMonth - 1,
          monthDays[prevMonth] - weekDay + i + 1
          ));
          // 這個月
        } else if (i < weekDay + monthDays[curMonth]) {
          calendarDateArray.push(new Date(curYear,
          curMonth,
          i + 1 - weekDay
          ));
          // 下個月
        } else {
          calendarDateArray.push(new Date(curYear,
          curMonth + 1,
          i + 1 - weekDay - monthDays[curMonth]
          ));
        }
      }
      return calendarDateArray;
    },
    isExpanded() {
      if (this.weekIndex == this.expandedWeek) {
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    expand() {
      this.$emit('changeExpandedWeek', this.weekIndex);
    }
  }
};
</script>

<style>
.week {
    display: grid;
    grid-template: auto / repeat(7, 1fr)
}
</style>