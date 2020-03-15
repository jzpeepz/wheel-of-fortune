<template>
  <div id="letter-panel">
    <div
      class="letter-panel-item"
      v-for="letter in letters"
      :key="letter"
      v-text="letter"
      @click="clicked(letter, $event)"
    ></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      letters: [
        'a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm',
        'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z',
      ],
    };
  },
  methods: {
    clicked(letter, event) {
      const { target } = event;
      const { textIndent } = target.style;

      if (textIndent.length === 0 || textIndent === '0px') {
        // make letter visible
        target.style.textIndent = '-99999em';
      } else {
        // hide letter
        target.style.textIndent = '0px';
      }
      this.$emit('letter-panel-clicked', letter);
    },
  },
};
</script>

<style lang="scss">
#letter-panel {
  background-color: #000;
}
.letter-panel-item {
  display: inline-block;
  text-transform: uppercase;
  font-size: 1.5rem;
  font-weight: 700;
  color: #2c3e50;
  padding:5px 15px;
  cursor: pointer;
  user-select: none;

  &:hover {
    background-color: rgb(75, 75, 75);
    color: #fff;
  }
}
</style>
