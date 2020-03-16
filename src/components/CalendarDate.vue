<template>
    <div class="date  cursor-pointer" :class="isToday">
      <div class="date-number">{{ date.getDate() }}</div>
      <transition name="date-fade">
        <p v-show="isExpanded">{{ date }}</p>
      </transition>
    </div>
</template>

<script>
export default {
  name: "CalendarDate",
  props: {
    weekMonth: Number,
    date: Date,
    isExpanded: Boolean
  },
  data: function() {
    return {
    };
  },
  computed: {
    isToday() {
      let today = new Date();
      if (this.date.getFullYear() == today.getFullYear() && this.date.getMonth() == today.getMonth() && this.date.getDate() == today.getDate()) {
        return "current-day";
      } else if (this.weekMonth == this.date.getMonth()) {
        return "";
      } else {
        return "color";
      }
    }
  }
};
</script>

<style>
.week {
    display: grid;
    grid-template: auto / repeat(7, 1fr)
}
.color {
    color: #FEFDFD;
    background-color: rgb(39,60,86);
}
.date {
    height: auto;
    padding: 5px; 
    border: 1px solid rgb(39,60,86);
    border-radius: 10px;
    margin: 5px 5px;
    box-shadow: 1px 1px 2px rgb(39,60,86);
}

.date:hover {
    box-shadow: 5px 5px 3px rgb(39,60,86);
    transition: border .2s ease;
}
.date:active {
    background-color: rgb(108,166,214);
    box-shadow: 5px 5px 3px rgb(39,60,86);
    transition: border .1s ease;
}
.current-day {
  background-color: rgb(200, 225, 255);
}
.date-number {
  font-size: 16px;
}
.date-fade-enter-active {
  overflow: hidden;
  transition: all .5s ease;
}
.date-fade-leave-active {
  overflow: hidden;
  transition: all .5s ease;
}
.date-fade-enter, .date-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  height: 0;
}

</style>