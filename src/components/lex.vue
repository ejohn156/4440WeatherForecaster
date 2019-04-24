<template>
  <div>
    <!-- <img
      v-on:click="clicked"
      src="https://github.com/awslabs/aws-lex-browser-audio-capture/blob/master/example/lex.png?raw=true"
    > -->
  
  <amplify-chatbot v-bind:chatbotConfig="this.chatbot"></amplify-chatbot>

  </div>
</template>

<script>
import { AmplifyEventBus } from "aws-amplify-vue";
import awsmobile from "../aws-exports"
export default {
  name: "lex",
  data: function(){
    chatbot = {
      bot: "BookTrip_forlex",
      //botName: "BookTrip",
      conversationModeOn: true,
      voiceConfig: { silenceDetectionConfig: { time: 2000, amplitude: 0.2 } },
      voiceEnabled: true,
      textEnabled: false
    },
    click = false
  },
  methods: {
    clicked() {
      this.click = true
    }
  },
  created(){
    var name = awsmobile.aws_bots_config[0].name
    this.chatbot = {
      bot: name,
      //botName: "BookTrip",
      conversationModeOn: true,
      voiceConfig: { silenceDetectionConfig: { time: 2000, amplitude: 0.2 } },
      voiceEnabled: true,
      textEnabled: false
    }
  }
  
};
</script>