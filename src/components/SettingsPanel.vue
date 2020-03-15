<template>
  <div id="settings-panel">
    <img id="settings-toggle" src="/img/sliders-h-light.svg" alt="Settings" @click="toggleModal">
    <div id="settings-modal" v-show="visible" @click.self="toggleModal">
      <div id="settings-modal-inner">
        <div class="form-group">
          <label>Puzzle</label>
          <textarea v-model="puzzle"></textarea>
        </div>
        <div class="form-group">
          <label>Category</label>
          <select v-model="category">
            <option
              v-for="category in categories"
              :key="category"
              v-html="category"
              :value="category"
            ></option>
          </select>
        </div>
        <div style="display: flex;">
          <div style="width: 50%; padding-right: 5px;">
            <button class="btn-primary" @click.prevent="updateSettings">Update Settings</button>
          </div>
          <div style="width: 50%; padding-left: 5px;">
            <button @click.prevent="toggleModal">Cancel</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      visible: false,
      puzzle: '',
      category: '',
      categories: [
        'Around the House', 'Before & After', 'Best Seller(s)', 'Character(s)', 'Classic TV', 'College Life',
        'Event(s)', 'Family', 'Fun & Games', 'Food & Drink', 'Headline', 'Husband & Wife', 'In the Kitchen',
        'Landmark(s)', 'Living Thing(s)', 'Movie Quote', 'TV Quote', 'Occupation(s)', 'On the Map', 'Person',
        'People', 'Phrase(s)', 'Place(s)', 'Proper Name(s)', 'Quotation', 'Rhyme Time', 'Same Letter',
        'Same Name', 'Show Biz', 'Song/Artist', 'Song Lyrics', 'Star & Role', 'Thing(s)', 'Title(s)',
        'Title/Author', 'What Are You Doing?', 'What Are You Wearing?',
      ],
    };
  },
  methods: {
    updateSettings() {
      this.$emit('settings-changed', {
        puzzle: this.puzzle,
        category: this.category,
      });

      this.toggleModal();
    },
    toggleModal() {
      this.visible = !this.visible;
    },
  },
};
</script>

<style lang="scss" scoped>
#settings-panel {
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  align-items: center;
  justify-content: flex-end;
}
#settings-toggle {
  width: 4vw;
  height: 4vw;
  margin-right: 2vw;
  cursor: pointer;
}
#settings-modal {
  position: fixed;
  top: 0px;
  right: 0px;
  bottom: 0px;
  left: 0px;
  background-color: rgba(0, 0, 0, 0.75);
  display: flex;
  align-items: center;
  justify-content: center;
}
#settings-modal-inner {
  width: 80vw;
  // height: 80vh;
  background-color: #222;
  border-radius: 3px;
  text-align: left;
  padding: 2rem;
  .form-group {
    margin-bottom: 15px;
    label {
      display: block;
    }
    input, textarea, select {
      display: block;
      width: 100%;
      font-size: 1.25rem;
      padding: 1.25rem;
    }
    textarea {
      height: 10rem;
    }
  }
  button {
    width: 100%;
    padding: 10px;
    border-radius: 4px;
    font-size: 1rem;
    cursor: pointer;

    &.btn-primary {
      background-color: #300560;
      color: #fff;
    }
  }
}
</style>
