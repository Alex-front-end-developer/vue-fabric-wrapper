<template>
  <div id="app">
    <h1>Vue Fabric Wrapper: Examples</h1>
    <h2>StickMan</h2>
    <stick-man />
    <h2>Animation</h2>
    <animation />
    <h2>Line Grid</h2>
    <line-grid />
    <h2>Dot Grid</h2>
    <dot-grid />
    <fabric-canvas
      :id="'events'"
      :height="400"
      :width="400"
      @object-moved="objMoved"
      @drop="objDropped"
      @after-render="afterRender"
      @canvas-updated="canvasUpdated"
      ref="canvas"
    >
      <fabric-rectangle :id="'rectGridTesting2'"></fabric-rectangle>

      <fabric-rectangle :id="'rectGridTesting1'"></fabric-rectangle>
      <fabric-rectangle :id="'rectGridTesting3'"></fabric-rectangle>
    </fabric-canvas>
    {{ text }}
    {{ textOriginal }}
    <div class="item">
      <label>
        <input type="radio" value="none" v-model="textTransform" />
        none
      </label>
      <label>
        <input type="radio" value="uppercase" v-model="textTransform" />
        uppercase
      </label>
      <label>
        <input type="radio" value="capitalize" v-model="textTransform" />
        capitalize
      </label>
      <label>
        <input type="radio" value="lowercase" v-model="textTransform" />
        lowercase
      </label>
      <label>
        <input type="checkbox" v-model="linethrough" />
        linethrough
      </label>
    </div>
    <div id="drag1" @dragstart="dragStart" class="draggable" draggable></div>
    {{ canvas }}
    <button @click="setText">image</button>
    {{ image }}
    <fabric-canvas :id="'overlay'" :height="400" :width="400">
      <fabric-background-image
        :id="345667"
        :scaleX="400 / imgWidth"
        :scaleY="400 / imgHeight"
        @image-loaded="imageLoaded"
      ></fabric-background-image>
      <fabric-group :id="3421">
        <fabric-polygon :id="342101">
          <!--          <fabric-gradient
            :x1="0"
            :x2="0"
            :y1="0"
            :y2="1"
            :gradientUnits="'percentage'"
            :colorStops="{ 0: '#000', 1: '#fff' }"
          ></fabric-gradient>
          <fabric-shadow
            :offsetX="25"
            :offsetY="25"
            :color="'rgba(0,0,0,.3)'"
          ></fabric-shadow>-->
        </fabric-polygon>
        <fabric-text :id="342104" :text="'hello stick man'"></fabric-text>
      </fabric-group>

      <fabric-text-box
        :id="'text'"
        :text.sync="text"
        :textOriginal.sync="textOriginal"
        :linethrough="linethrough"
        :textTransform="textTransform"
        :top="200"
        :left="200"
        :_controlsVisibility="{
          bl: true,
          br: true,
          mb: true,
          ml: true,
          mr: true,
          mt: true,
          mtr: true,
          tl: true,
          tr: true
        }"
      ></fabric-text-box>

      <fabric-polyline :id="342102"></fabric-polyline>
      <fabric-svg-from-url :id="'ttyuud'"></fabric-svg-from-url>
      <fabric-svg-from-url :id="'ttyuud' + newId"></fabric-svg-from-url>
      <fabric-overlay-image
        :id="3455667"
        :scaleX="400 / overImgWidth"
        :scaleY="400 / overImgHeight"
        @image-loaded="overImageLoaded"
      ></fabric-overlay-image>
    </fabric-canvas>
    <b-form-input v-model="newId"></b-form-input>
    <fabric-canvas :id="'image-update-test'">
      <FabricImageFromUrl :id="'ttyeeuud'" :url="imageUrl"></FabricImageFromUrl>
      <fabric-svg-from-url
        :id="'ttyuqqqqqud'"
        :url="svgUrl"
        :left.sync="svgLeft"
        :top.sync="svgTop"
      ></fabric-svg-from-url>
    </fabric-canvas>
    <b-button @click="imageInvert">invert</b-button>
  </div>
</template>

<script>
import StickMan from "./examples/StickMan";
import Animation from "./examples/Animation";
import LineGrid from "./examples/LineGrid";
import DotGrid from "./examples/DotGrid";
import FabricAnimation from "./components/FabricAnimation";
import FabricBackgroundImage from "./components/FabricBackgroundImage";
import FabricOverlayImage from "./components/FabricOverlayImage";
import FabricCanvas from "./components/FabricCanvas";
import FabricCircle from "./components/FabricCircle";
import FabricEllipse from "./components/FabricEllipse";
import FabricGradient from "./components/FabricGradient.vue";
import FabricGroup from "./components/FabricGroup";
import FabricImageFromUrl from "./components/FabricImageFromURL";
import FabricLine from "./components/FabricLine";
import FabricPolygon from "./components/FabricPolygon";
import FabricPolyline from "./components/FabricPolyine";
import FabricText from "./components/FabricText";
import FabricTextBox from "./components/FabricTextBox";
import FabricSvgFromUrl from "./components/FabricSVGFromURL";
import FabricTriangle from "./components/FabricTriangle";
import FabricRectangle from "./components/FabricRectangle";
import FabricShadow from "./components/FabricShadow";

export default {
  name: "App",
  components: {
    StickMan,
    Animation,
    LineGrid,
    DotGrid,
    FabricBackgroundImage,
    FabricOverlayImage,
    FabricCanvas,
    /*FabricGradient,*/
    FabricGroup,
    FabricPolygon,
    FabricPolyline,
    FabricSvgFromUrl,
    FabricImageFromUrl,
    FabricText,
    /*FabricShadow,*/
    FabricTextBox
  },
  data() {
    return {
      startAnimation: false,
      image: "",
      canvas: null,
      newId: "hello",
      imageUrl: "../vue.png",
      svgUrl: "../svg/loaderColor.svg",
      svgLeft: 20,
      svgTop: 30,
      imgHeight: 1,
      imgWidth: 1,
      overImgHeight: 1,
      overImgWidth: 1,
      text: "test TextBox flags",
      textOriginal: "test TextBox flags",
      textTransform: "none",
      linethrough: false
    };
  },
  methods: {
    objMoved(e) {
      console.log("objMoved", e);
    },
    imageLoaded(img) {
      this.imgHeight = img.height;
      this.imgWidth = img.width;
    },
    overImageLoaded(img) {
      this.overImgHeight = img.height;
      this.overImgWidth = img.width;
    },
    objDropped(e) {},
    dragStart(e) {
      e.dataTransfer.setData("text/plain", "mmmmmeeeee");
    },
    afterRender(e) {},
    canvasUpdated(c) {
      this.canvas = c;
    },
    setText() {
      this.image = this.canvas.toDataURL({
        format: "jpeg",
        quality: 0.8
      });
    },
    imageInvert() {
      if (this.imageUrl == "../vue.png") {
        this.imageUrl = "../vue-down.png";
        this.svgUrl = "../svg/pipe-end.svg";
      } else {
        this.imageUrl = "../vue.png";
        this.svgUrl = "../svg/pipe.svg";
      }
    }
  }
};
</script>
<style lang="scss">
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.draggable {
  display: inline-block;
  width: 100px;
  background-color: purple;
  height: 100px;
}
</style>
