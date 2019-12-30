<template>
    <div class="timer">
        <div class="columns">
          <div class="column">
            <div v-on:click.stop="dropdown_toggle($event)" class="dropdown" id="pomodoro-dropdown">
              <div class="dropdown-trigger">
                <button class="button is-info" aria-haspopup="true" aria-controls="dropdown-menu">
                  <span>Select interval</span>
                  <span class="icon is-small">
                    <font-awesome-icon :icon="toggleAngle" />
                    <!-- <i class="fas fa-angle-down" aria-hidden="true"></i> -->
                  </span>
                </button>
              </div>
              <div class="dropdown-menu" id="dropdown-menu" role="menu">
                <div class="dropdown-content">
                  <a class="dropdown-item" v-for="item in items" :key="item.id" @click="select_interval(item)">
                    {{item.name}}
                  </a>
                </div>
              </div>
            </div>
          </div>
            <div class="column" id="timer">
            <p v-if="selected_interval == null ">00:00</p>
            <p v-else>{{selected_interval}}</p>
          </div> 
          <div class="column">
            <button :class="{'is-start': isStart, 'is-stop': isStop}" class="button" @click="startTimer">{{buttonText}}</button>  
          </div>           
        </div>
    </div>
</template>

<script>
// import moment from 'moment'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faAngleDown } from '@fortawesome/free-solid-svg-icons'
import { faAngleUp } from '@fortawesome/free-solid-svg-icons'

export default {
  name: 'Timer',
  props: {
    msg: String
  },
  components:{
    FontAwesomeIcon
  },
  data: function(){
    return {
      selected_interval: null,
      intervalID: null,
      buttonText: "Start",
      isStart: true,
      isStop: false,
      toggleAngle: faAngleDown,
      items: [
      {
          id: 1,
          name: "10 minutes"
      },
      {
          id: 2,
          name: "15 minutes"
      },
      {
          id: 3,
          name: "20 minutes"
      },
      {
        id: 4,
        name: "25 minutes"
      }
      ]
    }
  },
  methods: {
    dropdown_toggle: function(event) {
      event.stopPropagation()
      let dropdown = document.querySelector('#pomodoro-dropdown');
      dropdown.classList.toggle("is-active")
      if(this.toggleAngle == faAngleDown){
        this.toggleAngle = faAngleUp
      }
      else{
        this.toggleAngle = faAngleDown
      }
    },
    select_interval: function(item) {
      // alert(item.name)
      this.selected_interval = item.name.split(" ")[0] + ":00"
      // let time_interval_ms = parseInt(this.selected_interval) * 60 * 100
      // alert(time_interval_ms)
    },
    startTimer: function(){
      if(this.selected_interval == null){
        alert("select interval")
      }
      else{
        let delay = 1000
        let repetitions = parseInt(this.selected_interval.split(":")[0]) * 60
        this.isStart = !this.isStart
        this.isStop = !this.isStop
        this.buttonText = this.isStart? "Start" : "Stop"
        if(this.buttonText == "Start"){
          clearInterval(this.intervalID);
          this.selected_interval = null
        }
        else{
          this.setInterval_(this.updateTimer, delay, repetitions)
        }
      }
    },
    updateTimer: function(timeRemaining){
      let minutes = parseInt(timeRemaining / 60) == 0 ? "00" : parseInt(timeRemaining / 60).toString()
      let seconds = parseInt(timeRemaining % 60) == 0 ? "00" : parseInt(timeRemaining % 60).toString()

      this.selected_interval = minutes + ":" + seconds
    },
    setInterval_: function(callback, delay, repetitions) {
      let x = 0;
       this.intervalID = setInterval(function () {

       callback(repetitions - x);

       if (++x === repetitions) {
           clearInterval(this.intervalID);
       }
      }, delay)
    }
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css?family=Courier+Prime&display=swap');

.timer > *, span, .button {
  font-family: 'Courier Prime', monospace;
}

/* .is-primary{
    background-color : #4c3cfc;
    color: #fcfcfc;
    font-family: 'Courier Prime', monospace;
}

.is-danger{
    background-color : #d32418;
    color: #fcfcfc;
    font-family: 'Courier Prime', monospace;
} */

#timer {
  font-family: 'Courier Prime', monospace;
  font-size: 50px;
}

.button.is-start {
  background-color: #03A6A6;
}

.button.is-stop {
  background-color: #da491e;
}

</style>


