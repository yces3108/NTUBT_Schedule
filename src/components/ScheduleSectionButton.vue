<template>
  <h4
    class="section-description"
    @dblclick="toggle"
  >
    {{ description }}
  </h4>

</template>

<script>
export default {
  name: "ScheduleSectionButton",
  props: {
    description: String,
    index: Number,
  },
  methods: {
    toggle() {
      this.$emit('toggleThisSection', this.index);
    }
  }
};
</script>

<style>
.section-description {
  height: 30px;
  cursor: pointer;
  vertical-align: middle;
}
</style>