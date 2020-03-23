<template>
<div>
  <div 
    :class="{
      positionButton: true,
      pitcherActive: isPitcher,
      pitcherNonactive: !isPitcher,
    }"
    @click="changePositionToPithcer"
  >
    投
  </div>
  <div 
    :class="{
      positionButton: true,
      catcherActive: highlightedPosition==2,
      catcherNonactive: highlightedPosition!=2,
    }"
    @click="changePositionToCatcher"
  >
    捕
  </div>
  <div 
    :class="{
      positionButton: true,
      infielderActive: highlightedPosition==3,
      infielderNonactive: highlightedPosition!=3,
    }"
    @click="changePositionToInfielder"
  >
    內
  </div>
  <div 
    :class="{
      positionButton: true,
      outfielderActive: highlightedPosition==4,
      outfielderNonactive: highlightedPosition!=4,
    }"
    @click="changePositionToOutfielder"
  >
    外
  </div>
</div>
</template>

<script>
export default {
  name: "PositionSwitch",
  props: {
    highlightedPosition: Number
  },
  data: function () {
    return {
      positions: [1, 2, 3, 4]
    }
  }, 
  computed: {
    isPitcher() {
      return this.highlightedPosition == 1;
    }
  },
  methods: {
    changePositionTo(position) {
      this.$emit("changePosition", position);
    },
    changePositionToPithcer() {
      this.$emit("changePosition", 1);
    },
    changePositionToCatcher() {
      this.$emit("changePosition", 2);
    },
    changePositionToInfielder() {
      this.$emit("changePosition", 3);
    },
    changePositionToOutfielder() {
      this.$emit("changePosition", 4);
    }
  }
}
</script>

<style>
.positionButton {
  cursor: pointer;
  display: inline-block;
  list-style-type: none;
  width: 20px;
  margin: 5px 10px;
  border-radius: 5px; 
  padding: 5px 10px;
  font-size:  1.2rem;
  text-align: center;
}
.pitcherActive {
  border: 3px solid rgb(255, 75, 144);
  background-color: rgb(255, 75, 144);
  color: white;
}
.pitcherNonactive {
  border: 3px solid rgb(255, 75, 144);
  color: rgb(39, 60, 86);
}
.catcherActive {
  border: 3px solid rgb(0, 99, 228);
  background-color: rgb(0, 99, 228);
  color: white;
}
.catcherNonactive {
  border: 3px solid rgb(0, 99, 228);
  color: rgb(39, 60, 86);
}
.infielderActive {
  border: 3px solid rgb(255, 239, 93);
  background-color: rgb(255, 239, 93);
  color: rgb(39, 60, 86);
}
.infielderNonactive {
  border: 3px solid rgb(255, 239, 93);
  color: rgb(39, 60, 86);
}
.outfielderActive {
  border: 3px solid rgb(51, 156, 2);
  background-color: rgb(51, 156, 2);
  color: white;
}
.outfielderNonactive {
  border: 3px solid rgb(51, 156, 2);
  color: rgb(39, 60, 86);
}
</style>