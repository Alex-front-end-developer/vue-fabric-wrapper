<script>
import fabricObject from "./fabricObject";
export default {
  name: "fabric-text-box",
  mixins: [fabricObject],
  props: {
    top: {
      type: Number,
      default: 40
    },
    left: {
      type: Number,
      default: 0
    },
    fontFamily: {
      type: String,
      default: "Times New Roman"
    },
    fontSize: {
      type: Number,
      default: 40
    },
    fontStyle: {
      type: String,
      default: "normal"
    },
    fontWeight: {
      type: [Number, String],
      default: "normal"
    },
    text: {
      type: String,
      required: true
    },
    textOriginal: {
      type: String
    },
    textTransform: {
      type: String,
      default: "none"
    },
    textAlign: {
      type: String,
      default: "left"
    },
    lineHeight: {
      type: Number,
      default: 1.16
    },
    charSpacing: {
      type: Number,
      default: 0
    },
    linethrough: {
      type: Boolean,
      default: false
    },
    underline: {
      type: Boolean,
      default: false
    },
    overline: {
      type: Boolean,
      default: false
    },
    textBackgroundColor: String
  },
  data() {
    return {
      textObj: null,
      type: "text"
    };
  },
  render(h) {
    return this.$slots.default ? h("div", this.$slots.default) : undefined;
  },
  watch: {
    parentItem: {
      handler(newValue) {
        if (newValue) {
          //Parent is created
          this.textObj = new this.fabric.Textbox(this.text, {
            ...this.definedProps
          });
          let currTextObj = this.textObj.toObject();
          this.textObj.toObject = () => {
            return {
              ...currTextObj,
              ...this.definedProps
            };
          };
          let textTransform = {
            none: this.text,
            uppercase: this.text.toUpperCase(),
            lowercase: this.text.toLowerCase(),
            capitalize: this.text.replace(/^([a-z])|\s+([a-z])/g, $1 => {
              return $1.toUpperCase();
            })
          };
          this.textObj.text = textTransform[this.textTransform];
          if (this.parentType === "group") {
            if (this.parentItem.addWithoutUpdate) {
              this.parentItem.add(this.textObj);
            } else {
              this.parentItem.addWithUpdate(this.textObj);
            }
          } else {
            this.canvas.add(this.textObj);
          }
          this.createEvents();
          this.createWatchers();
        }
      },
      immediate: true
    },
    text(text) {
      if (text.length > this.textOriginal.length) {
        this.$emit(
          "update:textOriginal",
          this.textOriginal + text.slice(this.textOriginal.length)
        );
      } else {
        this.$emit(
          "update:textOriginal",
          this.textOriginal.slice(0, text.length)
        );
      }
      let textTransform = {
        none: text,
        uppercase: text.toUpperCase(),
        lowercase: text.toLowerCase(),
        capitalize: this.text.replace(/^([a-z])|\s+([a-z])/g, $1 => {
          return $1.toUpperCase();
        })
      };
      this.$emit("update:text", textTransform[this.textTransform]);
    },
    textTransform() {
      let textTransform = {
        none: this.textOriginal,
        uppercase: this.textOriginal.toUpperCase(),
        lowercase: this.textOriginal.toLowerCase(),
        capitalize: this.text.replace(/^([a-z])|\s+([a-z])/g, $1 => {
          return $1.toUpperCase();
        })
      };
      this.$emit("update:text", textTransform[this.textTransform]);
    }
  },
  methods: {},
  beforeDestroy() {}
};
</script>
