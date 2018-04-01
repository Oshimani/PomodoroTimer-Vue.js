/* eslint-disable */
<template>
  <div class="timer">
    <h1 v-bind:class="{'time-over': timeOver}">{{ minutes }}:{{ seconds }}</h1>
    <font-awesome-icon class="icon icon-reset" :icon="iconUndoAlt"  v-on:click="resetTimer()"/>    
    <font-awesome-icon v-if="timerIsRunning" class="icon" :icon="iconPause"  v-on:click="pauseTimer()"/>
    <font-awesome-icon v-else class="icon" :icon="iconPlay"  v-on:click="startTimer()"/>
  </div>
</template>

<script>
import FontAwesomeIcon from "@fortawesome/vue-fontawesome";
import {
  faPlay,
  faPause,
  faUndoAlt
} from "@fortawesome/fontawesome-free-solid";
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
  created: function() {
    // init intervall
    this.intervallId = setInterval(() => {
      if (this.duration.asSeconds() > 0 && this.timerIsRunning)
        this.duration.subtract(1, "second");
    }, 1000);
  },
  methods: {
    startTimer: function() {
      console.log("Start Timer");
      this.timerIsRunning = true;
    },
    pauseTimer: function() {
      console.log("Pause Timer");
      this.timerIsRunning = false;
    },
    resetTimer: function() {
      console.log("Reset Timer");
      this.duration = moment.duration(25, "Minutes");
    }
  },
  computed: {
    minutes: function() {
      return ("0" + this.duration.minutes()).slice(-2);
    },
    seconds: function() {
      return ("0" + this.duration.seconds()).slice(-2);
    },
    timeOver: function() {
      if (this.duration.asSeconds() <= 0) return true;
      return false;
    },

    // Icons
    iconPlay() {
      return faPlay;
    },
    iconPause() {
      return faPause;
    },
    iconUndoAlt() {
      return faUndoAlt;
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
  user-select: none;
}
.time-over {
  color: #c82333;
}
.icon {
  cursor: pointer;
  font-size: 50px;
}
.icon:hover {
  color: #1a2530;
  transform: scale(1.2);
}
.icon-reset {
  margin-right: 50px;
}
</style>
