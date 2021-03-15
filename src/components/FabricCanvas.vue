<template>
  <div>
    <canvas :id="id"></canvas>
    <fabric-line
      v-for="(line, i) in verticalTop"
      :key="'verticalTop' + i"
      :id="'verticalTop' + i"
      :x1="line.x1"
      :y1="line.y1"
      :x2="line.x2"
      :y2="line.y2"
      :stroke="aligningGrid.strokeColor"
      :strokeWidth="aligningGrid.strokeWidth"
      :selectable="false"
    ></fabric-line>
    <fabric-line
      v-for="(line, i) in verticalMiddle"
      :key="'verticalMiddle' + i"
      :id="'verticalMiddle' + i"
      :x1="line.x1"
      :y1="line.y1"
      :x2="line.x2"
      :y2="line.y2"
      :stroke="aligningGrid.strokeColor"
      :strokeWidth="aligningGrid.strokeWidth"
      :selectable="false"
    ></fabric-line>
    <fabric-line
      v-for="(line, i) in verticalBottom"
      :key="'verticalBottom' + i"
      :id="'verticalBottom' + i"
      :x1="line.x1"
      :y1="line.y1"
      :x2="line.x2"
      :y2="line.y2"
      :stroke="aligningGrid.strokeColor"
      :strokeWidth="aligningGrid.strokeWidth"
      :selectable="false"
    ></fabric-line>
    <fabric-line
      v-for="(line, i) in horizontalLeft"
      :key="'horizontalLeft' + i"
      :id="'horizontalLeft' + i"
      :x1="line.x1"
      :y1="line.y1"
      :x2="line.x2"
      :y2="line.y2"
      :stroke="aligningGrid.strokeColor"
      :strokeWidth="aligningGrid.strokeWidth"
      :selectable="false"
    ></fabric-line>
    <fabric-line
      v-for="(line, i) in horizontalMiddle"
      :key="'horizontalMiddle' + i"
      :id="'horizontalMiddle' + i"
      :x1="line.x1"
      :y1="line.y1"
      :x2="line.x2"
      :y2="line.y2"
      :stroke="aligningGrid.strokeColor"
      :strokeWidth="aligningGrid.strokeWidth"
      :selectable="false"
    ></fabric-line>
    <fabric-line
      v-for="(line, i) in horizontalRight"
      :key="'horizontalRight' + i"
      :id="'horizontalRight' + i"
      :x1="line.x1"
      :y1="line.y1"
      :x2="line.x2"
      :y2="line.y2"
      :stroke="aligningGrid.strokeColor"
      :strokeWidth="aligningGrid.strokeWidth"
      :selectable="false"
    ></fabric-line>
    <slot></slot>
  </div>
</template>

<script>
import FabricLine from "@/components/FabricLine";

let canvasEvents = [
  //Static Canvas events
  "before:render",
  "after:render",
  "canvas:cleared",
  "object:added",
  "object:removed",
  //Canvas events
  "object:modified",
  "object:rotated",
  "object:scaled",
  "object:moved",
  "object:skewed",
  "object:rotating",
  "object:scaling",
  "object:moving",
  "object:skewing",
  "before:transform",
  "before:selection:cleared",
  "selection:cleared",
  "selection:updated",
  "selection:created",
  "path:created",
  "mouse:down",
  "mouse:move",
  "mouse:up",
  "mouse:down:before",
  "mouse:move:before",
  "mouse:up:before",
  "mouse:over",
  "mouse:out",
  "mouse:dblclick",
  "dragover",
  "dragenter",
  "dragleave",
  "drop"
];

import * as fabric from "fabric";
import fabricStaticCanvas from "./fabricStaticCanvas";

export default {
  name: "FabricCanvas",
  components: {
    FabricLine
  },
  mixins: [fabricStaticCanvas],
  props: {
    id: { type: String, required: false, default: "c" },
    aligningGrid: {
      type: Object,
      default: () => {
        return {
          visible: true,
          strokeColor: "rgba(102,153,255,0.7)",
          strokeWidth: 1
        };
      }
    },
    altActionKey: { type: String, required: false, default: "shiftKey" },
    // altSelectionKey,
    centeredKey: { type: String, required: false, default: "altKey" },
    centeredRotation: { type: Boolean, required: false, default: false },
    centeredScaling: { type: Boolean, required: false, default: false },
    containerClass: {
      type: String,
      required: false,
      default: "canvas-container"
    },
    defaultCursor: { type: String, required: false, default: "default" },
    fireMiddleClick: { type: Boolean, required: false, default: false },
    fireRightClick: { type: Boolean, required: false, default: false },
    freeDrawingCursor: { type: String, required: false, default: "crosshair" },
    hoverCursor: { type: String, required: false, default: "move" },
    isDrawingMode: { type: Boolean, required: false, default: false },
    moveCursor: { type: String, required: false, default: "move" },
    notAllowedCursor: { type: String, required: false, default: "not-allowed" },
    perPixelTargetFind: { type: Boolean, required: false, default: false },
    preserveObjectStacking: { type: Boolean, required: false, default: false },
    rotationCursor: { type: String, required: false, default: "crosshair" },
    selection: { type: Boolean, required: false, default: true },
    selectionBorderColor: {
      type: String,
      required: false,
      default: "rgba(255, 255, 255, 0.3)"
    },
    selectionColor: {
      type: String,
      required: false,
      default: "rgba(100, 100, 255, 0.3)"
    },
    // selectionDashArray: {
    //   type: Array,
    //   required: false,
    //   default: () => []
    // },
    selectionFullyContained: { type: Boolean, required: false, default: false },
    selectionKey: { type: String, required: false, default: "shiftKey" },
    selectionLineWidth: { type: Number, required: false, default: 1 },
    skipTargetFind: { type: Boolean, required: false, default: false },
    snapAngle: { type: Number, required: false, default: 0 },
    // snapThreshold,
    stopContextMenu: { type: Boolean, required: false, default: false },
    targetFindTolerance: { type: Number, required: false, default: 0 },
    // targets
    uniScaleKey: { type: String, required: false, default: "shiftKey" },
    uniformScaling: { type: Boolean, required: false, default: false },

    backgroundColor: { type: String, required: false, default: "" },
    width: { type: Number, required: false, default: 600 },
    height: { type: Number, required: false, default: 400 }
  },
  data() {
    return {
      canvas: null,
      fabric: fabric,
      type: "canvas",
      contactPointsVertical: new Map(),
      contactPointsHorizontal: new Map(),
      verticalTop: [],
      verticalMiddle: [],
      verticalBottom: [],
      horizontalLeft: [],
      horizontalMiddle: [],
      horizontalRight: [],
      objectMoving: null,
      mousePointer: null,
      sticky: {}
    };
  },
  provide() {
    return {
      $canvas: () => this.canvas,
      $group: () => null,
      fabric
    };
  },
  methods: {
    clearLines() {
      this.verticalTop = [];
      this.verticalMiddle = [];
      this.verticalBottom = [];
      this.horizontalLeft = [];
      this.horizontalMiddle = [];
      this.horizontalRight = [];
    },
    createEvents() {
      canvasEvents.forEach(event => {
        let vueEvent = event.split(":").join("-");
        this.canvas.on(event, e => {
          this.$emit(vueEvent, e);
        });
      });
    },
    createLine(point, x1, y1, contactPoints, contactLines) {
      let contactPoint = this[contactPoints].get(Math.round(point));
      if (contactPoint) {
        this[contactLines] = [];
        this.objectMoving.set({
          left: Math.round(this.objectMoving.left),
          top: Math.round(this.objectMoving.top)
        });
        contactPoint.forEach(line => {
          this[contactLines].push({
            x1: Math.round(x1),
            y1: Math.round(y1),
            x2: line.left,
            y2: line.top
          });
        });
        if (!this.sticky.left) {
          this.sticky = {
            left: Math.round(this.objectMoving.left),
            x: this.mousePointer.x,
            top: Math.round(this.objectMoving.top),
            y: this.mousePointer.y
          };
        }
        /*if (contactPoints === "contactPointsVertical" && !this.sticky.top) {
          this.sticky = {
            top: Math.round(this.objectMoving.top),
            y: this.mousePointer.y
          };
          console.log(contactPoints, this.sticky);
        }*/
      } else {
        this[contactLines] = [];
      }
    },
    checkSnap(e) {
      if (!this.aligningGrid.visible) return;
      this.objectMoving = e.target;
      this.mousePointer = e.pointer;

      if (this.sticky.top && Math.abs(e.pointer.y - this.sticky.y) < 15) {
        this.objectMoving.set({
          top: this.sticky.top
        });
      } else {
        this.sticky = {};
      }

      if (this.sticky.left && Math.abs(e.pointer.x - this.sticky.x) < 15) {
        this.objectMoving.set({
          left: this.sticky.left
        });
      } else {
        this.sticky = {};
      }

      this.createLine(
        this.objectMoving.top,
        this.objectMoving.left,
        this.objectMoving.top,
        "contactPointsVertical",
        "verticalTop"
      );
      this.createLine(
        this.objectMoving.top +
          this.objectMoving.height * this.objectMoving.scaleY,
        this.objectMoving.left,
        this.objectMoving.top +
          this.objectMoving.height * this.objectMoving.scaleY,
        "contactPointsVertical",
        "verticalMiddle"
      );
      this.createLine(
        this.objectMoving.top +
          (this.objectMoving.height * this.objectMoving.scaleY) / 2,
        this.objectMoving.left,
        this.objectMoving.top +
          (this.objectMoving.height * this.objectMoving.scaleY) / 2,
        "contactPointsVertical",
        "verticalBottom"
      );

      this.createLine(
        this.objectMoving.left,
        this.objectMoving.left,
        this.objectMoving.top,
        "contactPointsHorizontal",
        "horizontalLeft"
      );
      this.createLine(
        this.objectMoving.left + this.objectMoving.width,
        this.objectMoving.left + this.objectMoving.width,
        this.objectMoving.top,
        "contactPointsHorizontal",
        "horizontalMiddle"
      );
      this.createLine(
        this.objectMoving.left +
          (this.objectMoving.width * this.objectMoving.scaleX) / 2,
        this.objectMoving.left +
          (this.objectMoving.width * this.objectMoving.scaleX) / 2,
        this.objectMoving.top,
        "contactPointsHorizontal",
        "horizontalRight"
      );
    },
    finishMove() {
      this.objectMoving = null;
      this.clearLines();
    },
    hashTable(getSet, left, top, contactPoints) {
      if (!this[contactPoints].get(getSet)) {
        this[contactPoints].set(Math.round(getSet), [
          {
            left: left,
            top: top
          }
        ]);
      } else {
        this[contactPoints].get(getSet).push({
          left: left,
          top: top
        });
      }
    },
    createContactPoints(e) {
      this.contactPointsVertical.clear();
      this.contactPointsHorizontal.clear();
      this.canvas.getObjects().forEach(obj => {
        if (obj !== e.target) {
          this.hashTable(
            obj.left,
            obj.left,
            obj.top,
            "contactPointsHorizontal"
          );
          this.hashTable(
            obj.left + obj.width * obj.scaleX,
            obj.left + obj.width * obj.scaleX,
            obj.top,
            "contactPointsHorizontal"
          );
          this.hashTable(
            obj.left + (obj.width * obj.scaleX) / 2,
            obj.left + (obj.width * obj.scaleX) / 2,
            obj.top,
            "contactPointsHorizontal"
          );

          this.hashTable(obj.top, obj.left, obj.top, "contactPointsVertical");
          this.hashTable(
            obj.top + obj.height * obj.scaleY,
            obj.left,
            obj.top + obj.height * obj.scaleY,
            "contactPointsVertical"
          );
          this.hashTable(
            obj.top + (obj.height * obj.scaleY) / 2,
            obj.left,
            obj.top + (obj.height * obj.scaleY) / 2,
            "contactPointsVertical"
          );
        }
      });
    }
  },
  computed: {
    definedProps() {
      const obj = { ...this.$props };
      Object.keys(obj).forEach(key => {
        if (obj[key] === undefined) {
          delete obj[key];
        }
      });
      return obj;
    },
    canvasExists() {
      if (typeof this.canvas === typeof undefined) {
        return false;
      }
      if (this.canvas == null) {
        return false;
      }
      return true;
    }
  },
  mounted() {
    this.canvas = new fabric.Canvas(this.id, {
      ...this.definedProps
    });
    this.createEvents();
    this.canvas.on("selection:created", e => this.createContactPoints(e));
    this.canvas.on("selection:updated", e => this.createContactPoints(e));
  },
  beforeDestroy() {
    canvasEvents.forEach(event => {
      let vueEvent = event.split(":").join("-");
      this.canvas.off(event, this.$emit(vueEvent));
    });
  },
  watch: {
    canvas: {
      handler(newValue) {
        this.$emit("canvas-updated", this.canvas);
      },
      deep: true,
      initial: true
    },
    height(newValue) {
      this.canvas.setHeight(newValue);
      this.canvas.renderAll();
      this.canvas.calcOffset();
    },
    width(newValue) {
      this.canvas.setWidth(newValue);
      this.canvas.renderAll();
      this.canvas.calcOffset();
    },
    backgroundColor(newValue) {
      this.canvas.setBackgroundColor(newValue, () => {
        this.canvas.renderAll();
      });
    },
    overlayColor(newValue) {
      this.canvas.setOverlayColor(newValue, () => {
        this.canvas.renderAll();
      });
    },
    canvasExists: {
      handler(newValue) {
        if (newValue) {
          this.canvas.on("object:moved", () => {
            this.finishMove();
          });
          this.canvas.on("object:moving", e => {
            this.checkSnap(e);
          });
        }
      },
      immediate: true
    }
  }
};
</script>

<style>
canvas {
  border: 1px solid;
}
</style>
