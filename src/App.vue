<template>
  <div id="app">
    <div class="top-container">
      <button class="new-note" @click="addNote">+ New Note</button>
      <div>Color:</div>
      <button
        class="color-button"
        :style="{ background: '#28abe3' }"
        @click="selected !== 0 ? setChildColor(selected - 1, '#28abe3') : null"
      />
      <button
        class="color-button"
        :style="{ background: '#f05b24' }"
        @click="selected !== 0 ? setChildColor(selected - 1, '#f05b24') : null"
      />
      <button
        class="color-button"
        :style="{ background: '#8cc73e' }"
        @click="selected !== 0 ? setChildColor(selected - 1, '#8cc73e') : null"
      />
      <button
        class="color-button"
        :style="{ background: '#f6931f' }"
        @click="selected !== 0 ? setChildColor(selected - 1, '#f6931f') : null"
      />
    </div>

    <div class="note-container">
      <note
        :key="note"
        ref="noteRef"
        v-for="note in noteCount"
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
      noteCount: 0,
      selected: 0,
    };
  },
  mounted() {
    if (localStorage.noteCount) {
      this.noteCount = localStorage.noteCount;
      this.noteCount++;
    }
  },
  methods: {
    addNote() {
      localStorage.noteCount = this.noteCount;
      this.noteCount++;
    },
    onSelectedUpdate(newData) {
      this.selected = newData;
    },
    setChildColor(index, newColor) {
      this.$refs.noteRef[index].setColor(newColor);
    },
  },
};
</script>
<style lang="scss">
@import "./styles/app.scss";
</style>