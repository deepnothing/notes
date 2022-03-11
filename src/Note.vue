<template>
  <vue-resizable
    class="resizable-component"
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
        background: color,
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
  props: ["indexValue", "selectedValue", "selectedColor"],
  components: {
    VueResizable,
  },
  data() {
    const tW = 200;
    const tH = 200;
    return {
      text: "",
      color: "#28abe3",
      zIndex: 0,
      handlers: ["r", "rb", "b", "lb", "l", "lt", "t", "rt"],
      left: `110px`,
      top: `${210 * this.indexValue}px`,
      height: tH,
      width: tW,
      minW: 100,
      minH: 100,
      event: "",
      dragSelector: ".drag-container",
    };
  },
  beforeMount() {
    //retreive text
    if (localStorage.getItem(`${this.indexValue}text`)) {
      this.text = localStorage.getItem(`${this.indexValue}text`);
    }
    //retreive  color
    if (localStorage.getItem(`${this.indexValue}color`)) {
      this.color = localStorage.getItem(`${this.indexValue}color`);
    }
    //sretreive  size
    if (localStorage.getItem(`${this.indexValue}width`)) {
      this.width = localStorage.getItem(`${this.indexValue}width`) + "px";
    }
    if (localStorage.getItem(`${this.indexValue}height`)) {
      this.height = localStorage.getItem(`${this.indexValue}height`) + "px";
    }
    //sretreive  position
    if (localStorage.getItem(`${this.indexValue}left`)) {
      this.left = localStorage.getItem(`${this.indexValue}left`) - -8 + "px";
    }
    if (localStorage.getItem(`${this.indexValue}top`)) {
      this.top = localStorage.getItem(`${this.indexValue}top`) - -38 + "px";
    }
  },
  methods: {
    handleInput(event) {
      this.text = event.target.value;
      localStorage.setItem(`${this.indexValue}text`, this.text);
    },
    setSelected() {
      //pass note index up to parent to select note
      this.$emit("update", this.indexValue);

      //click through handler element
      this.$refs.textarea.focus();
    },
    setColor(newColor) {
      this.color = newColor;
      localStorage.setItem(`${this.indexValue}color`, newColor);
    },

    //vue resize anddrag handler function
    eHandler(data) {
      //set new size and positions on drag
      this.width = data.width;
      this.height = data.height;
      this.left = data.left;
      this.top = data.top;
      this.event = data.eventName;

      //set position in localstorage
      localStorage.setItem(`${this.indexValue}width`, data.width);
      localStorage.setItem(`${this.indexValue}height`, data.height);
      //set size in localstorage
      localStorage.setItem(`${this.indexValue}left`, data.left);
      localStorage.setItem(`${this.indexValue}top`, data.top);
    },
  },
};
</script>

<style lang="scss">
@import "./styles/note.scss";
</style>
