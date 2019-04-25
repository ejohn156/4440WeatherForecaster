<template>
  <div>
    <v-container grid-list-x1>
      <v-card style="background-color:#1b5e20;">
        <v-card-title primary-title>
          <h1
            class="headline mb-0"
            style="color:gold"
          >{{weather.datetime}} Weather Simulation</h1>
        </v-card-title>
        <v-layout row>
          <vue-p5 v-on:sketch="sketch" v-on:setup="setup" style="margin-left:10%;margin-bottom: 4%"></vue-p5>
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
  props: {
    weather: Object,
    temp: Number,
    precip: Number,
    cloudCoverage: Number
  },
  data: () => ({
    red: 255,
    green: 0,
    blue: 0,
    lines: [],
    temperature: 0,
    precipitationRate: 0,
    clouds: 0
  }),
  mounted() {
    this.setup;
  },
  methods: {
    sketch(sketch) {
      sketch.draw = () => {
        if (this.$props.temp <= 0) {
          sketch.background(192, 192, 192);
        } else if (this.$props.temp > 0 && this.$props.temp <= 5) {
          sketch.background(165, 242, 243);
        } else if (this.$props.temp > 5 && this.$props.temp <= 10) {
          sketch.background(32, 213, 119);
        } else if (this.$props.temp > 10 && this.$props.temp <= 16) {
          sketch.background(173, 255, 47);
        } else if (this.$props.temp > 16 && this.$props.temp <= 21) {
          sketch.background(255, 255, 153);
        } else if (this.$props.temp > 21 && this.$props.temp <= 27) {
          sketch.background(255, 255, 0);
        } else if (this.$props.temp > 27 && this.$props.temp <= 32) {
          sketch.background(255, 165, 0);
        } else if (this.$props.temp > 32 && this.$props.temp <= 37) {
          sketch.background(255, 69, 0);
        } else if (this.$props.temp > 37) {
          sketch.background(178, 34, 34);
        }

        sketch.fill(102);
        sketch.rect(-10, 350, 900, 63);
        var clouds = Math.floor(this.$props.cloudCoverage / 2)
        for (var i = 0; i < clouds; i++) {
          var y = Math.floor(Math.random() * 50) + 20;
          var x = Math.floor(Math.random() * 850) + 0;
          sketch.fill("rgb(230,230,250)");
          sketch.ellipse(x, y, 80, 50);
        }
        var numDrops = this.$props.precip * 10
        for (var j = 0; j < numDrops; j++) {
          var ry = Math.floor(Math.random() * 300) + 50;
          var rx = Math.floor(Math.random() * 850) + 0;
          sketch.fill("rgb(0,0,139)");
          sketch.ellipse(rx, ry, 2, 8);
        }
      };
    },
    setup(sketch) {
      sketch.createCanvas(850, 400);
    }
  }
};
</script>



