<template>
  <div
    v-text="revealed && object !== null ? object.value : ''"
    :class="classObject"
    @click="reveal"
  ></div>
</template>

<script>
export default {
  props: {
    object: {
      type: Object,
      default: null,
    },
  },
  mounted() {
    // this.object = this.initial;
  },
  computed: {
    classObject() {
      return {
        'slot-used': this.object !== null && this.object.value.trim().length > 0,
        'slot-unused': this.object === null || this.object.value.trim().length === 0,
        'slot-highlighted': this.highlighted,
      };
    },
  },
  watch: {
    object(newObject) {
      this.revealed = newObject.revealed;
      this.highlighted = newObject.highlighted;
    },
  },
  data() {
    return {
      revealed: false,
      highlighted: false,
    };
  },
  methods: {
    reveal() {
      this.revealed = !this.revealed;
      this.highlighted = false;
    },
  },
};
</script>

<style lang="scss">

</style>
