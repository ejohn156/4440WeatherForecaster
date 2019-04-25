<template>
  <div>
    <v-container grid-list-x1>
      <v-card style="background-color:#1b5e20;">
        <v-card-title primary-title>
          <h1
            class="headline mb-0"
            style="color:gold"
          >Weather Simulation Precip: {{precip}} Temp: {{this.temperature}} cloudCoverage: {{cloudCoverage}} Description:{{weather.weather.description}} </h1>
       </v-card-title> 
        <v-layout row>
    <vue-p5 
        v-on:sketch="sketch"
        v-on:setup="setup" 
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
  mounted(){
    this.setup
  },
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

  },
  
};
</script>



