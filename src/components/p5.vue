<template>
  <div>
    <v-container grid-list-x1>
      <v-card style="background-color:#1b5e20;">
        <v-card-title primary-title>
          <h1
            class="headline mb-0"
            style="color:gold"
          >Weather Simulation Precip: {{precip}} Temp: {{temp}} cloudCoverage: {{cloudCoverage}} Description:{{weather.weather.description}} </h1>
       </v-card-title> 
        <v-layout row>
    <vue-p5 
        v-on:sketch="sketch"
        v-on:setup="setup" 
        v-on:draw="draw"
        v-on:keypressed="keyPressed"
        v-on:mousemoved="mouseMoved"
        v-on:mousedragged="mouseDragged"
        style="margin-left:10%;margin-bottom: 4%">
    </vue-p5>
    <p>
      Red: {{ red }} <br/>
      Green: {{ green }} <br/>
      Blue: {{ blue }} <br/>
    </p>
    <p>
      Press <button v-on:click="toggleRed()">button</button> to toggle red color <br/>
      Press <code>g</code> to toggle green color <br/>
      Use mouse to draw lines <br/>
    </p>
        </v-layout>
      </v-card>
    </v-container>

  </div>
</template>

<script>
import VueP5 from "vue-p5";
export default {
  name: "sim",
  components: {
    "vue-p5": VueP5
  },
  data: () => ({
    red: 255,
    green: 0,
    blue: 0,
    lines: [],
    temperature: 0
  }),
  methods: {
    sketch(sketch) {
      sketch.draw = () => {
        this.blue = (this.blue + 3) % 255;
        const { red, green, blue } = this;
        sketch.background(red, green, blue);
      };
    },
    setup(sketch) {
      sketch.createCanvas(850, 400);
    },
    draw(sketch) {
      const { width, height } = sketch;
      sketch.image(this.backgroundImage, 0, 0, 0.5 * width, 0.5 * height);
      for (let line of this.lines) {
        sketch.stroke(line.color);
        sketch.line(line.pmouseX, line.pmouseY, line.mouseX, line.mouseY);
      }
    },
    keyPressed({ keyCode }) {
      // 'g' key
      if (keyCode === 71) {
        this.toggleGreen();
      }
    },
    mouseMoved({ mouseX, mouseY, pmouseX, pmouseY }) {
      this.pushLine({ mouseX, mouseY, pmouseX, pmouseY, color: 0 });
    },
    mouseDragged({ mouseX, mouseY, pmouseX, pmouseY }) {
      this.pushLine({ mouseX, mouseY, pmouseX, pmouseY, color: 255 });
    },
    toggleRed() {
      this.red = 255 - this.red;
    },
    toggleGreen() {
      this.green = 255 - this.green;
    },
    pushLine(line) {
      let lines = this.lines;
      lines.push(line);
      this.lines = lines.slice(-100);
    }
  },
  mounted(){
    this.setup
    this.temperature = this.props.temp
  },
  props: {
    weather: Object,
    temp: Float32Array,
    precip: Float32Array,
    cloudCoverage: Float32Array
  }
};
</script>



