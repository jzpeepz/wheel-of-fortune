<template>
  <div id="wheel-board">
    <div id="board-slots">
      <BoardSlot
        v-for="slot in slots"
        :key="slot.index"
        :object="slot"
        class="board-slot"
      ></BoardSlot>
    </div>
    <div id="category">
      <h1 v-text="category"></h1>
      <SettingsPanel
        v-on:settings-changed="updateSettings"
      ></SettingsPanel>
    </div>
    <LetterPanel
      @letter-panel-clicked="highlightLetter"
    ></LetterPanel>
  </div>
</template>

<script>
import Vue from 'vue';

import SettingsPanel from '@/components/SettingsPanel.vue';
import LetterPanel from '@/components/LetterPanel.vue';
import BoardSlot from '@/components/BoardSlot.vue';

export default {
  components: {
    SettingsPanel,
    LetterPanel,
    BoardSlot,
  },
  data() {
    return {
      slots: [],
      text: '',
      category: '',
    };
  },
  mounted() {
    this.fillSlots();
    this.updateSlots();
  },
  methods: {
    fillSlots() {
      this.slots = [];
      for (let index = 0; index < 56; index += 1) {
        Vue.set(this.slots, index, {
          value: '',
          index,
          revealed: false,
          highlighted: false,
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

      // update slots
      Array.from(this.text).forEach((element, index) => {
        Vue.set(this.slots, index, {
          value: element,
          index,
          revealed: false,
          highlighted: false,
        });
      });
    },
    updateSettings(settings) {
      this.text = settings.puzzle;
      this.category = settings.category;
      this.fillSlots();
      this.updateSlots();
    },
    highlightLetter(letter) {
      this.slots = this.slots.map((slot) => {
        const newSlot = JSON.parse(JSON.stringify(slot));
        if (slot.value.toLowerCase() === letter) {
          newSlot.highlighted = !slot.highlighted;
          return newSlot;
        }
        return slot;
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
#board-slots {
  width: 100vw;
  display: grid;
  grid-template-columns: repeat(14, 7.14%);
  grid-template-rows: repeat(4, calc(100vw/14*1.368));
  background-color: #fff;
  border: 4px solid #000;
}
#board-slots > * {
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
    // background-color: #adff2f;
  }

  &.slot-used {
    cursor:pointer;
  }

  &.slot-unused {
    background-color: green;
  }

  &.slot-highlighted {
    background-color: rgb(175, 219, 255);
  }
}
#category {
  background-color: #300560;
  color: #fff;
  padding: 15px;
  position: relative;
  min-height: 75px;
  h1 {
    margin: 0;
    padding: 0;
  }
}
</style>
