<template>
  <div id="app">
    <div class="top-container">
      <button class="new-note" @click="addNote">+ New Note</button>
      <div>
        Notes:<strong>{{ notes }}</strong> &nbsp;
      </div>
      <div>Color:</div>
      <button
        v-for="i in colors"
        :key="i"
        class="color-button"
        :style="{ background: i }"
        @click="selected !== 0 ? setChildColor(selected - 1, i) : null"
      />
    </div>

    <div class="note-container">
      <note
        :key="note"
        ref="noteRef"
        v-for="note in notes"
        :index-value="note"
        :selected-value="selected"
        @update="onSelectedUpdate"
      />
    </div>
  </div>
</template>

<script>
import Note from "./Note.vue";

export default {
  name: "App",
  components: {
    Note,
  },

  data() {
    return {
      selected: 0,
      notes: localStorage.length,
      colors: ["#28abe3", "#f05b24", "#8cc73e", "#f6931f"],
    };
  },

  methods: {
    addNote() {
      localStorage.setItem(
        (this.notes += 1),
        JSON.stringify({
          text: "",
          color: "#28abe3",
          width: 200,
          height: 200,
          left: `110px`,
          top: `210px`,
        })
      );
    },
    onSelectedUpdate(newData) {
      //set selected note
      this.selected = newData;
    },
    setChildColor(index, newColor) {
      //trigger function in child component
      this.$refs.noteRef[index].setColor(newColor);
    },
  },
};
</script>
<style lang="scss">
@import "./styles/app.scss";
</style>