<template>
  <div id="wheel-board">
    <div
      v-for="slot in slots"
      v-text="slot.value"
      :key="slot.index"
      class="board-slot"
      :class="{'slot-unused': slot.state === 'unused'}"
    ></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      slots: [],
      text: 'where the deer and the antelope play video games',
    };
  },
  mounted() {
    this.fillSlots();
    this.updateSlots();
  },
  methods: {
    fillSlots() {
      for (let index = 0; index < 56; index += 1) {
        this.slots.push({
          value: '',
          index,
          state: 'unused',
        });
      }
    },
    updateSlots() {
      // let letters = Array.from(this.text);
      const words = this.text.split(' ');

      const lines = [];
      let current = '';
      while (words.length > 0) {
        const newWord = words.shift();
        if ((current.length + newWord.length) > 14) {
          // move to a new line
          lines.push(current.trim());
          current = `${newWord} `;
        } else {
          // append to current line
          current += `${newWord} `;
        }

        if (words.length === 0) {
          lines.push(current.trim());
          current = '';
        }
      }

      const longestLine = lines.reduce((a, b) => (a.length > b.length ? a : b));

      const lengthOfLongestLine = longestLine.length;

      const lineDiff = 14 - lengthOfLongestLine;

      const startPadding = Math.floor(lineDiff / 2);

      const finalLines = lines.map((line) => {
        let newLine = line;
        newLine = newLine.padStart(startPadding + newLine.length, ' ');
        newLine = newLine.padEnd(14, ' ');
        return newLine;
      });

      if (finalLines.length < 3) {
        finalLines.unshift('              ');
      }

      this.text = finalLines.join('');

      Array.from(this.text).forEach((element, index) => {
        this.slots[index].value = element;
        if (element === ' ') {
          this.slots[index].state = 'unused';
        } else {
          this.slots[index].state = 'used';
        }
      });
    },
  },
};
</script>

<style lang="scss">
html, body {
  padding: 0;
  margin: 0;
  background-color: #000;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
}
#wheel-board {
  width: 100vw;
  display: grid;
  grid-template-columns: repeat(14, 7.14%);
  grid-template-rows: repeat(4, calc(100vw/14*1.368));
  background-color: #fff;
}
#wheel-board > * {
  border: 4px solid #000;
}
.board-slot {
  display: flex;
  justify-content: center;
  align-items: center;
  text-transform: uppercase;
  font-size: 50px;
  font-weight: 900;
  color: #000;

  &.board-slot:hover {
    background-color: #adff2f;
  }

  &.slot-unused {
    background-color: green;
  }
}
</style>
