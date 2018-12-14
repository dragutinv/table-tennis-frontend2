<template>
  <div class="score-board">
    <div id="header"></div>
    <div class="row">
      <div class="col-md-6 playerName" v-if="oddSets == 0">{{ playerOneName }}</div>
      <div class="col-md-6 playerName" v-if="oddSets == 0">{{ playerTwoName }}</div>
      <div class="col-md-6 playerName" v-if="oddSets == 1">{{ playerTwoName }}</div>
      <div class="col-md-6 playerName" v-if="oddSets == 1">{{ playerOneName }}</div>
    </div>
    <div class="row">
      <div class="col-md-6 playerScore" v-if="oddSets == 0">{{ playerOneScore }}</div>
      <div class="col-md-6 playerScore" v-if="oddSets == 0">{{ playerTwoScore }}</div>
      <div class="col-md-6 playerScore" v-if="oddSets == 1">{{ playerTwoScore }}</div>
      <div class="col-md-6 playerScore" v-if="oddSets == 1">{{ playerOneScore }}</div>
    </div>
    <div class="row">
      <div class="col-md-6 playerSet" v-if="oddSets == 0"><span v-for="i in playerOneSets" class="setImage"><img src="../../static/images/set.png">{{ i }}</span></div>
      <div class="col-md-6 playerSet" v-if="oddSets == 0"><span v-for="i in playerTwoSets" class="setImage"><img src="../../static/images/set.png">{{ i }}</span></div>
      <div class="col-md-6 playerSet" v-if="oddSets == 1"><span v-for="i in playerTwoSets" class="setImage"><img src="../../static/images/set.png">{{ i }}</span></div>
      <div class="col-md-6 playerSet" v-if="oddSets == 1"><span v-for="i in playerOneSets" class="setImage"><img src="../../static/images/set.png">{{ i }}</span></div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'ScoreBoard',
  data () {
    return {
      playerOneName: 'Fernando',
      playerTwoName: 'Simen',
      playerOneScore: '0',
      playerTwoScore: '0',
      playerOneSets: '0',
      playerTwoSets: '0',
      winner: '',
      serveSide: '',
      oddSets: 0
    }
  },
  mounted: function () {
    setInterval(this.fetchResult, 1000)
  },
  methods: {
    fetchResult: function () {
      axios({
        url: 'http://10.12.122.188:8080/result',
        method: 'get',
        timeout: 1000
      }).then(function (response) {
        this.playerOneScore = response.data.playerOneScore
        this.playerTwoScore = response.data.playerTwoScore
        this.playerOneSets = response.data.playerOneSets
        this.playerTwoSets = response.data.playerTwoSets
        this.winner = response.data.winner
        this.serveSide = response.data.serveSide

        if ((this.playerOneSets + this.playerTwoSets) % 2 === 1) {
          this.oddSets = 1
        } else {
          this.oddSets = 0
        }
      }.bind(this)).catch(function (error) {
        console.log(error)
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style>
  body{
    background-color: #e3e4e4!important;
    font-family: "Source Sans Pro";
  }
  #app{
    margin-top: 0;
  }
  #header {
    border-bottom: 3px solid #00003c;
    min-height: 3em;
    background: #00003c url('../../static/images/logo.png') center bottom no-repeat;
    font-size: 2em;
    margin-bottom: 10px;
  }
  h1, h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }

  .playerName {
    text-align: center;
    font-size: 4em;
  }

  .playerScore {
    text-align: center;
    font-size: 25em;
    color:#00003c;
    border-right: 1px solid gray;
  }
  .playerSet {
    text-align: center;
    font-size: 6em;
    color:transparent;
  }
  .setImage img{
    width: 0.7em;
  }
</style>
