<template>
  <div id="app">
    
    <div class="row">
      <button id="green" v-on:click="captureTap('green')" v-bind:class="{ 'lightGreen': lightGreen}" class="button green space">GREEN</button>
      <button id="red" v-on:click="captureTap('red')" v-bind:class="{ 'lightRed': lightRed}" class="button red space">RED</button>
    </div>
    <div class="row">
      <button id="blue" v-on:click="captureTap('blue')" v-bind:class="{ 'lightBlue': lightBlue}" class="button blue space">YELLOW</button>
      <button id="yellow" v-on:click="captureTap('yellow')" v-bind:class="{ 'lightYellow': lightYellow}" class="button yellow space">BLUE</button>
    </div>
  
    <button v-on:click="start" class="btn btn-md start">Start</button>


  </div>
</template>

<script>
  export default {
    name: 'app',


    data () {
      return {
        longest: 0,
        sequence:[],
        taps:[],
        lights: [ 'green', 'red', 'yellow', 'blue'],
        lightGreen: false,
        lightBlue: false,
        lightRed: false,
        lightYellow:false,
        litCounter: 0,
        currentColor: '',
        simonFinished: true
      }
    },
    computed: {
      current: function() {
        return this.sequence.length;
      }
    },

    mounted(){

    },

 methods: {
    start: function() {
      this.sequence = ["blue"];
      this.addToSequence();
      this.playSequence();
      //this.startTimer();
    },

    chooseRandomLight: function() {
      var index = Math.floor(Math.random() * 4);
      console.log(index);
      return this.lights[index];
    },

    addToSequence: function() {
      this.sequence.push(this.chooseRandomLight());
      console.log(this.sequence)
    },

    playSequence: function() {
        setTimeout(function () {  

            switch (this.sequence[[this.litCounter]]) {
              case "green":
                  this.lightGreen = true;
                  setTimeout(function () { this.lightGreen = false }.bind(this), 1000);
                  break;
              case "red":
                  this.lightRed = true;
                  setTimeout(function () { this.lightRed = false }.bind(this), 1000);
                  break;
              case "yellow":
                  this.lightYellow = true;
                  setTimeout(function () { this.lightYellow = false }.bind(this), 1000);
                  break;
              case "blue":
                  this.lightBlue = true;
                  setTimeout(function () { this.lightBlue = false }.bind(this), 1000);
                  break;
            };
            this.litCounter++;
            
            if (this.litCounter < this.sequence.length){
              this.playSequence();
            }
        }.bind(this), 2000);
    },

    captureTap: function(colorPressed) {
      if (simonFinished === true) {
     // console.log(colorPressed);
        this.taps.push(colorPressed);
        for (var i=0;i<this.sequence.length; i++) {
          console.log('sequence: ', this.sequence[i])
          for (var j=0; j<this.taps.length; j++ ) {
            console.log('taps: ', this.taps[j]);
            if (this.sequence[i] !== this.taps[j]) {
              alert("YOU LOSE");
            }
          }
        }
      }
    },
    startTimer: function() {
      alert('hi')
    },

  }
}




</script>

<style>
#app {
  margin-top: 10%;
  margin-left: 25%;
}
.start {

}

.green {
  opacity: 0.1;
  background-color: green;
  border: 1px solid black;
}

.red {
  opacity: 0.1;
  background-color: red;
  border: 1px solid red;
}

.yellow {
  opacity: 0.1;
  background-color: yellow;
  border: 1px solid yellow;
}

.blue {
  opacity: 0.1;
  background-color: blue;
  border: 1px solid blue;
}

.space {
  height: 150px;
  width: 150px;
  text-align: center;
  margin: 20px;

}

.bright {
  opacity: 1.0 !important;
},

.col {
  display: inline-block;
}

.lightGreen {
  opacity: 1.0;

}
.lightYellow {
  opacity: 1.0;

}
.lightRed {
  opacity: 1.0;

}
.lightBlue {
  opacity: 1.0;

}



</style>
