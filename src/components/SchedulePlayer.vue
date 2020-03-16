<template>
  <div @click="toggle">
    <p class="wrapper">
      {{ player }} / {{ position }} / {{ sections }} / {{ sections.length }} / {{ getlength }}
    </p>
  </div>
</template>

<script>
//import axios from "axios";
export default {
  name: "SchedulePlayer",
  props: {
    player: String,
    position: Object,
    sections: Array,
    strangeCount: Number
  },
  computed: {
    getlength() {
      return this.strangeCount + 500
    }
  },
  data: function () {
    return {
      days: [...Array(7).keys()]
    };
  },
  methods: {
    toggle() {
      this.$emit('updateSections', [this.player, this.day, this.section]);
      console.log("跑出什麼呢")
      console.log(this.sections)
    }
  },
  mounted: function() {
  }
};
</script>
