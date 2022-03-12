<template>
  <vue-resizable
    ref="resizableComponent"
    :dragSelector="dragSelector"
    :active="handlers"
    :min-width="minW"
    :min-height="minH"
    :width="width"
    :height="height"
    :top="top"
    :left="left"
    @resize:move="eHandler"
    @resize:start="eHandler"
    @resize:end="eHandler"
    @drag:move="eHandler"
    @drag:start="eHandler, (zIndex = 1)"
    @drag:end="eHandler, (zIndex = 0)"
    v-bind:style="{
      position: 'absolute',
      zIndex: this.zIndex,
      boxShadow:
        indexValue == selectedValue ? '0px 0px 10px 5px #FFE62B' : 'none',
    }"
    @click="setSelected"
  >
    <textarea
      class="drag-container"
      ref="textarea"
      v-bind:style="{
        background: this.color,
      }"
      placeholder="New Note"
      :value="text"
      @input="handleInput"
    ></textarea>
  </vue-resizable>
</template>
<script>
import VueResizable from "vue-resizable";

export default {
  name: "Note",
  props: ["indexValue", "selectedValue"],
  components: {
    VueResizable,
  },

  data() {
    return {
      text: this.noteData().text,
      color: this.noteData().color,
      left: this.noteData().left,
      top: this.noteData().top,
      height: this.noteData().height,
      width: this.noteData().width,
      minW: 100,
      minH: 100,
      zIndex: 0,
      handlers: ["r", "rb", "b", "lb", "l", "lt", "t", "rt"],
      event: "",
      dragSelector: ".drag-container",
    };
  },

  methods: {
    handleInput(event) {
      //set text
      this.text = event.target.value;

      //set localstrage
      localStorage.setItem(
        this.indexValue,
        JSON.stringify({ ...this.noteData(), text: event.target.value })
      );
    },

    setColor(newColor) {
      //set color
      this.color = newColor;

      //set localstorage
      localStorage.setItem(
        this.indexValue,
        JSON.stringify({ ...this.noteData(), color: newColor })
      );
    },

    //vue resize and drag handler function
    eHandler(data) {
      //set localstorage
      localStorage.setItem(
        this.indexValue,
        JSON.stringify({
          ...this.noteData(),
          width: `${data.width}px`,
          height: `${data.height}px`,
          left: `${data.left - -8}px`,
          top: `${data.top - -38}px`,
        })
      );
    },
    setSelected() {
      //pass note index up to parent to select note
      this.$emit("update", this.indexValue);

      //click through handler element
      this.$refs.textarea.focus();
    },
    noteData() {
      return JSON.parse(localStorage.getItem(this.indexValue));
    },
  },
};
</script>

<style lang="scss">
@import "./styles/note.scss";
</style>
