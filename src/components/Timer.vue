/* eslint-disable */
<template>
  <div class="timer">
    <h1 v-bind:class="{'time-over': timeOver}">{{ minutes }}:{{ seconds }}</h1>
    <font-awesome-icon class="icon" :icon="iconPlay"  v-on:click="startTimer()"/>
  </div>
</template>

<script>
import FontAwesomeIcon from "@fortawesome/vue-fontawesome";
import { faPlay } from "@fortawesome/fontawesome-free-solid";
var moment = require("moment");
export default {
  name: "Timer",
  data() {
    return {
      moment: moment,
      duration: moment.duration(25, "Minutes"),
      timerIsRunning: false,
      intervallId: 0
    };
  },
  methods: {
    startTimer: function() {
      if (this.timerIsRunning === false) {
        console.log("Start Timer");
        this.duration = moment.duration(25, "Minutes");
        this.intervallId = setInterval(() => {
          if (this.duration.asSeconds() > 0) {
            this.duration.subtract(1, "second");
          } else {
            clearInterval(this.intervallId);
            this.timerIsRunning = false;
          }
        }, 1000);
        this.timerIsRunning = true;
      }
    }
  },
  computed: {
    minutes: function() {
      return this.duration.minutes();
    },
    seconds: function() {
      return ("0" + this.duration.seconds()).slice(-2);
    },
    timeOver: function() {
      if (this.duration.asSeconds() <= 0) return true;
      return false;
    },
    iconPlay() {
      return faPlay;
    }
  },
  components: {
    FontAwesomeIcon
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h1 {
  font-size: 250px;
}
.time-over {
  color: #c82333;
}
.icon {
    cursor: pointer;
    font-size: 50px;
}
</style>
