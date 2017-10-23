<template>
  <div id="app">
    
    <div class="heading vuemon"> <img src="./assets/vue.png"> UE.mon  Says</div><br>
    <div class="subheading"><i>- a Simon Says clone, made with Vue.js</i></div>
    <div v-if="gameOver" class="card gameOverCard">
      <div class="card-block">
        <h3 class="card-title">{{ gameFinishedMessage }}</h3>
        <h6 class="card-subtitle mb-2 text-muted">{{ subtitleMessage }}</h6>
      </div>
    </div>
    <div class="streaktrun">
      <button v-on:click="newGame" v-if="newGameReady" class="btn btn-md btn-outline-primary newGame">New Game</button>
      <h5 class="streak">Your Best Streak: {{ longest }}</h5>
      <h5 class="streak">Current Streak: {{ currentStreak }}</h5>
      <div v-if="yourTurn === false" class = "card littlecard simonturn">VUE.mon TURN</div>
      <div v-if="yourTurn" class = "card littlecard yourturn">YOUR TURN</div>
    </div>
    <div class="gamebox">
      <div class="row">
        <div>
          <button id="green" v-on:click="captureTap('green')" v-bind:class="{ 'lightGreen': lightGreen}" class="button green space" style="outline:none;">
            <div class="innerGreen"></div>
          </button>
        </div>
          <button id="red" v-on:click="captureTap('red')" v-bind:class="{ 'lightRed': lightRed}" class="button red space" style="outline:none;">
            <div class="innerRed"></div>
          </button>
      </div>

      <div class="row">
        <button id="blue" v-on:click="captureTap('blue')" v-bind:class="{ 'lightBlue': lightBlue}" class="button blue space" style="outline:none;">
          <div class="innerBlue"></div>
        </button>
        <button id="yellow" v-on:click="captureTap('yellow')" v-bind:class="{ 'lightYellow': lightYellow}" class="button yellow space" style="outline:none;">
          <div class="innerYellow"></div>
        </button>
      </div>
    </div>
    
  
   


  </div>
</template>

<script>
  export default {
    name: 'app',


    data () {
      return {
        currentStreak: '0',  
        longest: '0',      
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
        tapped: 0,
        round: 0,
        yourTurn: true,
      }
    },


 methods: {
    newGame: function() {
      this.currentStreak = 0;
      this.taps = [];
      this.sequence = [];
      this.newGameReady = false;
      this.yourTurn= false;
      this.simonFinished = false;
      this.tapped = 0;
      this.gameOver = false;
      this.tapCounter = 0;
      this.litCounter = 0;
      this.round = 0;
      this.addToSequence();
      this.playSequence();


      
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
      if (!this.gameOver) {
        this.round = 0;
        this.currentStreak++;
        this.litCounter = 0;
        this.tapCounter = 0;
        this.taps = [];
        this.yourTurn = false;
        this.addToSequence();
        this.playSequence();
        if (this.currentStreak > this.longest) {
          this.longest++;
        }
        localStorage.setItem('this.longest', JSON.stringify(this.longest));
      }
    },

    playSequence: function() {
        setTimeout(function () {  
            switch (this.sequence[this.litCounter]) {
              case "green":
                  this.lightGreen = true;
                  setTimeout(function () { this.lightGreen = false }.bind(this), 300);
                  break;
              case "red":
                  this.lightRed = true;
                  setTimeout(function () { this.lightRed = false }.bind(this), 300);
                  break;
              case "yellow":
                  this.lightYellow = true;
                  setTimeout(function () { this.lightYellow = false }.bind(this), 300);
                  break;
              case "blue":
                  this.lightBlue = true;
                  setTimeout(function () { this.lightBlue = false }.bind(this), 300);
                  break;
            };
            this.litCounter++;
            if (this.litCounter < this.sequence.length){
              this.simonFinished = false;
              this.playSequence();
            }

            else if (this.litCounter >= this.sequence.length) {
              this.simonFinished = true;
              this.yourTurn = true;
              setTimeout(function () { this.startTimer(); }.bind(this), 20000);
            };
        }.bind(this), 1000);

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
          this.gameOver = true;
          this.gameFinishedMessage = "YOU LOSE!";
          this.subtitleMessage = "Better Luck Next Time..";
          this.yourTurn = '';
          this.newGameReady = true;
          this.simonFinished = false;
        }
        this.tapped++;    
        this.tapCounter++;
        if (this.tapCounter === this.sequence.length) {
          this.nextRound();
        }

      }
    },



    startTimer: function() {
      if ( this.round > this.tapped) {
          this.gameOver = true;
          this.gameFinishedMessage = "YOU LOSE!";
          this.subtitleMessage = 'Sorry, you took wayyyy too long!';
          this.newGameReady = true;
      }
    },
    // Put the object into storage

    // Retrieve the object from storage
    // var retrievedObject = localStorage.getItem('this.longest');

    // console.log('retrievedObject: ', JSON.parse(this.longest));

  },

  beforeMount: function() {
    var retrievedObject = localStorage.getItem('this.longest');
    this.longest = retrievedObject;
    console.log('retrievedObject: ', JSON.parse(this.longest));
  }

  
}


</script>

<style>

#app {
  margin-top: 5%;
  size: 50%;
}
.heading {
  font-size: 30px;
  margin-left:auto;
  margin-right:auto;
  width:315px;
  height:65px;
  color:#71d191;
  font-weight: bold;
  }

.gamebox {
  margin-left:auto;
  margin-right:auto;
  width:400px;
  height:400px;

}

.subheading {
  font-size: 14px;
  text-align: center;
  margin-left: 5%;
  color: red;
}

.row.green, .row.red {
  margin-left: 5%;
  margin-top: 5%;

}
.row {
  margin-left: 5%;
}

.gameOverCard {
  text-align: center;
  width:345px;
  margin-top: 1%;
  margin-left: auto;
  margin-right: auto;
  border: 1px solid red;
}

.card-title {
  font-size: 12px;
}
.streak {
  text-align: right;
  margin-right: 15%;
  color: #00b9f1;
}

.btn {
  float:left;
  margin-top: 1%;
  margin-left: 10%;
  color: blue;
}

.yourturn {
  border: 1px solid grey;
  color:#75D701;
  text-align: center;
  width: 150px;
  margin-left: 72%

}

.simonturn {
  border: 1px solid grey;
  text-align: center;
  width: 150px;
  margin-top: 1%;
  margin-left: 72%;
  color: red
}


.green {
  opacity: 0.5;
  background-color: #75D701;
  border: 1px solid white;
  border-radius: 80% 10% 0% 10%;
  position: relative;
}
.innerGreen {
  height:75px;
  width: 75px;
  position: absolute;
  bottom: 0;
  right: 0;
  border-radius: 80% 0% 0% 0%;
  background-color: white;
}

.red {
  opacity: 0.5;
  background-color: red;
  border: 1px solid white;
  border-radius: 10% 80% 10% 0%;
  position: relative;

}
.innerRed {
  height:75px;
  width: 75px;
  position: absolute;
  bottom: 0;
  left: 0;
  border-radius: 0% 80% 0% 0%;
  background-color: white;
}

.yellow {
  opacity: 0.4;
  background-color: #fbd14b;
  border: 1px solid white;
  border-radius: 0% 10% 80% 10%;
  position: relative;
}
.innerYellow {
  height:75px;
  width: 75px;
  position: absolute;
  top: 0;
  left: 0;
  border-radius: 0% 0% 80% 0%;
  background-color: white;
}

.blue {
  opacity: 0.5;
  background-color: #00b9f1;
  border: 1px solid white;
  border-radius: 10% 0% 10% 80%;
  position: relative;
}
.innerBlue {
  height:75px;
  width: 75px;
  position: absolute;
  top: 0;
  right: 0;
  border-radius: 0% 0% 0% 80%;
  background-color: white;
}

.space {
  height: 170px;
  width: 170px;
  margin: 2px;

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
