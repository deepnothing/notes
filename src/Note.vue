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
    const tW = 200;
    const tH = 200;
    const localData = localStorage.getItem(this.indexValue);

    return {     
      text: localData ? JSON.parse(localData).text : "",
      color: localData ? JSON.parse(localData).color : "#28abe3",
      left: localData ? JSON.parse(localData).left : `110px`,
      top: localData ? JSON.parse(localData).top : `${210 * this.indexValue}px`,
      height: localData ? JSON.parse(localData).height : tH,
      width: localData ? JSON.parse(localData).width : tW,
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
        JSON.stringify({
          text: event.target.value,
          color: this.color,
          width: this.width,
          height: this.height,
          left: this.left,
          top: this.top,
        })
      );
    },

    setColor(newColor) {
      //set color
      this.color = newColor;

      //set localstorage

      localStorage.setItem(
        this.indexValue,
        JSON.stringify({
          text:this.text,
          color: newColor,
          width: this.width,
          height: this.height,
          left: this.left,
          top: this.top,
        })
      );
    },
    //vue resize and drag handler function
    eHandler(data) {
      //set new size and positions on drag
      this.height = data.height;
      this.width = data.width;
      this.top = data.top;
      this.left = data.left;
      //set localstorage
      localStorage.setItem(
        this.indexValue,
        JSON.stringify({
          text: this.text,
          color: this.color,
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
  },
};
</script>

<style lang="scss">
@import "./styles/note.scss";
</style>
