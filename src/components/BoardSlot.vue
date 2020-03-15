<template>
  <div
    v-text="revealed ? text : ''"
    :class="classObject"
    @click="reveal"
  ></div>
</template>

<script>
export default {
  props: {
    text: {
      type: String,
      default: '',
    },
    initialState: {
      type: String,
      default: '',
    },
  },
  mounted() {
    this.state = this.initialState;
  },
  computed: {
    classObject() {
      return {
        'slot-used': this.text.trim().length > 0,
        'slot-unused': this.text.trim().length === 0,
        'slot-highlighted': this.highlighted,
      };
    },
  },
  data() {
    return {
      state: '',
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
