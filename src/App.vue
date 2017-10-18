<template>
  <div id="app">
    
    <div class="row">
      <button id="green" v-on:click="captureTap('green')" v-bind:class="{ 'lightGreen': lightGreen}" class="button green space"></button>
      <button id="red" v-on:click="captureTap('red')" v-bind:class="{ 'lightRed': lightRed}" class="button red space"></button>
    </div>
    <div class="row">
      <button id="blue" v-on:click="captureTap('blue')" v-bind:class="{ 'lightBlue': lightBlue}" class="button blue space"></button>
      <button id="yellow" v-on:click="captureTap('yellow')" v-bind:class="{ 'lightYellow': lightYellow}" class="button yellow space"></button>
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
        tapCounter: 0,
        lights: [ 'green', 'red', 'yellow', 'blue'],
        lightGreen: false,
        lightBlue: false,
        lightRed: false,
        lightYellow:false,
        litCounter: 0,
        currentColor: '',
        simonFinished: false,
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
              this.simonFinished = false;
              this.playSequence();
            }

            else if (this.litCounter >= this.sequence.length) {
              this.simonFinished = true;
            };
        }.bind(this), 2000);

    },

    captureTap: function(colorPressed) {
      if (this.simonFinished === true) {

        switch (colorPressed) {
              case "green":
                  this.lightGreen = true;
                  setTimeout(function () { this.lightGreen = false }.bind(this), 100);
                  break;
              case "red":
                  this.lightRed = true;
                  setTimeout(function () { this.lightRed = false }.bind(this), 100);
                  break;
              case "yellow":
                  this.lightYellow = true;
                  setTimeout(function () { this.lightYellow = false }.bind(this), 100);
                  break;
              case "blue":
                  this.lightBlue = true;
                  setTimeout(function () { this.lightBlue = false }.bind(this), 100);
                  break;
            };

        this.taps.push(colorPressed);
        if (this.sequence[[this.tapCounter]] !== this.taps[[this.tapCounter]]) {
          alert("YOU LOSE");
        }
        else {
          console.log('you are good for now')            
        }       
        this.tapCounter++;
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
