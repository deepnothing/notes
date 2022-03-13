<template>
  <div id="app">
    <div class="top-container">
      <button class="new-note" @click="addNote">+ New Note</button>
      <div>
        Notes:<strong>{{ notes }}</strong> &nbsp;
      </div>
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