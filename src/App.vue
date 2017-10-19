<template>
  <div id="app">
    <h1> V U E M O N</h1>
    <div v-if="gameOver" class="card gameOverCard">
      <div class="card-block">
        <h4 class="card-title">{{ gameFinishedMessage }}</h4>
        <h6 class="card-subtitle mb-2 text-muted">{{ subtitleMessage }}</h6>
      </div>
    </div>
    <div class="streak">
      <div class = "card">
        <div class="card-block">
          <h6 class="card-title">Current Streak: {{ currentStreak }}</h6>
        </div>
      </div>
      <div v-if="yourTurn === false" class = "card">
        <div class="card-block">
          <h6 class="card-title">VUEMON'S TURN</h6>
        </div>
      </div>
      <div v-if="yourTurn" class = "card">
        <div class="card-block">
          <h6 class="card-title">YOUR TURN!</h6>
        </div>
      </div>
    </div>

    <div class="row">
      <button id="green" v-on:click="captureTap('green')" v-bind:class="{ 'lightGreen': lightGreen}" class="button green space"></button>
      <button id="red" v-on:click="captureTap('red')" v-bind:class="{ 'lightRed': lightRed}" class="button red space"></button>
    </div>

    <div class="row">
      <button id="blue" v-on:click="captureTap('blue')" v-bind:class="{ 'lightBlue': lightBlue}" class="button blue space"></button>
      <button id="yellow" v-on:click="captureTap('yellow')" v-bind:class="{ 'lightYellow': lightYellow}" class="button yellow space"></button>
    </div>
  
    <button v-on:click="newGame" v-if="newGameReady" class="btn btn-md newGame">New Game</button>

   


  </div>
</template>

<script>
  export default {
    name: 'app',


    data () {
      return {
        currentStreak: '0',        
        sequence:[],
        taps:[],
        tapCounter: 0,
        lights: [ 'green', 'red', 'yellow', 'blue'],
        lightGreen: false,
        lightBlue: false,
        lightRed: false,
        lightYellow:false,
        litCounter: 0,
        simonFinished: false,
        newGameReady: true,
        gameFinishedMessage: '',
        subtitleMessage:'',
        gameOver: false,
        tapped: false,
        yourTurn: false,
      }
    },


 methods: {
    newGame: function() {
      this.gameOver = false;
      this.currentStreak = 0;
      this.addToSequence();
      this.playSequence();
      this.newGameReady = false;
      this.yourTurn= false
      
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

    nextRound: function() {
      this.currentStreak++;
      this.litCounter = 0;
      this.tapCounter = 0;
      this.taps = [];
      this.yourTurn = false;
      this.addToSequence();
      this.playSequence();
    },

    playSequence: function() {
        setTimeout(function () {  

            switch (this.sequence[[this.litCounter]]) {
              case "green":
                  this.lightGreen = true;
                  setTimeout(function () { this.lightGreen = false }.bind(this), 500);
                  break;
              case "red":
                  this.lightRed = true;
                  setTimeout(function () { this.lightRed = false }.bind(this), 500);
                  break;
              case "yellow":
                  this.lightYellow = true;
                  setTimeout(function () { this.lightYellow = false }.bind(this), 500);
                  break;
              case "blue":
                  this.lightBlue = true;
                  setTimeout(function () { this.lightBlue = false }.bind(this), 500);
                  break;
            };
            this.litCounter++;
            if (this.litCounter < this.sequence.length){
              this.simonFinished = false;
              this.playSequence();
            }

            else if (this.litCounter >= this.sequence.length) {
              this.simonFinished = true;
              this.tapped = false;
              setTimeout(function () { this.startTimer(); }.bind(this), 25000);
            };
        }.bind(this), 2000);

    },

    captureTap: function(colorPressed) {
      if (this.simonFinished === true) {
        this.yourTurn = true;
        this.tapped = true;
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
          this.gameOver = true;
          this.gameFinishedMessage = "YOU LOSE!";
          this.subTitleMessage = "Better Luck Next Time..";
          this.newGameReady = true;
          this.simonFinished = false;
        }    
        this.tapCounter++;
        if (this.tapCounter === this.sequence.length) {
          this.nextRound();
        }

      }
    },



    startTimer: function() {
      if ( this.tapped === false) {
          this.gameOver = true;
          this.gameFinishedMessage = "YOU LOSE!";
          this.subtitleMessage = 'Sorry, you too wayyyy too long!';
          this.newGameReady = true;
      }
    },

  }
}




</script>

<style>

#app {
  margin-top: 10%;
  margin-left: 25%;
}
h1 {
  margin-left: 10%;
}
.gameOverCard {
  text-align: center;
  width:345px;
}

.card {
  margin: 20px;
}
.streak {
  float: right;
  width: 200px;
  text-align: center;
  margin-right: 10%;
}


.green {
  opacity: 0.1;
  background-color: green;
  border: 2px solid black;
}

.red {
  opacity: 0.1;
  background-color: red;
  border: 2px solid black;
}

.yellow {
  opacity: 0.1;
  background-color: yellow;
  border: 2px solid black;
}

.blue {
  opacity: 0.1;
  background-color: blue;
  border: 2px solid black;
}

.space {
  height: 150px;
  width: 150px;
  text-align: center;
  margin: 20px;

}


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
