<template>
  <div>
    <v-container grid-list-x1>
      <v-card style="background-color:#1b5e20;">
        <v-card-title primary-title>
          <h1
            class="headline mb-0"
            style="color:gold"
          >Weather Simulation {{weather.weather.description}}</h1>
        </v-card-title>
        <v-layout row>
          <v-flex pa-12 style="align-content: center;padding-bottom: 3%">
            <vue-p5 v-on="this" style="margin-left: 8%" class="justify-center"></vue-p5>
          </v-flex>
        </v-layout>
      </v-card>
    </v-container>
  </div>
</template>
<script>
import vueP5 from "vue-p5";
export default {
  name: "sim",
  data: function(){
      return{
          temperature: 0
      }
  },
  methods: {
    setup(sketch) {
        //const clicks = [];
      sketch.createCanvas(900, 500);
      

      if(this.temp <= 0){
          sketch.background(165, 242, 243);
      }
      else if(this.temp > 0){
          sketch.background(255,255,0)
      }
    //   sketch.mouseClicked = () => {
    //     // save clicks to array
    //     clicks.push({ x: sketch.mouseX, y: sketch.mouseY });
    //   };

    //   sketch.draw = () => {
    //     // draw a circle around each clicked position
    //     clicks.forEach(({ x, y }) => {
    //       sketch.ellipse(x, y, 10, 10);
    //     });
    //   };
    }
    // setBackground(){
    //     if()
    // }
  },
  props: {
    weather: Object,
    temp: String
  },
  created(){
      this.temperature = this.props.temp
  },
  components: {
    "vue-p5": vueP5
  }
};
</script>
