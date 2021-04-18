<template>
  <div id="app">
    <div class="stats">
      <button class="start" v-on:click="startGame()">Старт</button>
      <span style="margin:4%"> Время: {{timerMin}}:{{timerSec}}</span>
      <span> Ход: {{turns}}</span>
    </div>
    <div class="cards">
      <div v-for="card in cards" v-bind:key="card.class" class="card-container">
        <div class="front" v-bind:class="{clicked: card.clicked, found: card.found}">
          <img src="./assets/0.png" v-on:click="showCard(card)">
        </div>
        <div class="back" v-bind:class="{clicked: card.clicked, found: card.found}">
          <img v-bind:src="getImgUrl(card.src)">
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      ClickedCards: 0,
      FoundedCards: 0,
      turns: 0,
      prevCard: null,
      prevCardClass: "",
      timerSec: "00",
      timerMin: 0,
      GameStage: 0,
      cards: [
        {class: "1",  src: "1.png"},
        {class: "1",  src: "1.png"},
        {class: "2",  src: "2.png"},
        {class: "2",  src: "2.png"},
        {class: "3",  src: "3.png"},
        {class: "3",  src: "3.png"},
        {class: "4",  src: "4.png"},
        {class: "4",  src: "4.png"},
        {class: "5",  src: "5.png"},
        {class: "5",  src: "5.png"},
        {class: "6",  src: "6.png"},
        {class: "6",  src: "6.png"},
        {class: "7",  src: "7.png"},
        {class: "7",  src: "7.png"},
        {class: "8",  src: "8.png"},
        {class: "8",  src: "8.png"},
        {class: "9",  src: "9.png"},
        {class: "9",  src: "9.png"},
        {class: "10",  src: "10.png"},
        {class: "10",  src: "10.png"},
        {class: "11",  src: "11.png"},
        {class: "11",  src: "11.png"},
        {class: "12",  src: "12.png"},
        {class: "12",  src: "12.png"},
        {class: "13",  src: "13.png"},
        {class: "13",  src: "13.png"},
        {class: "14",  src: "14.png"},
        {class: "14",  src: "14.png"},
        {class: "15",  src: "15.png"},
        {class: "15",  src: "15.png"},
        {class: "16",  src: "16.png"},
        {class: "16",  src: "16.png"},
        {class: "17",  src: "17.png"},
        {class: "17",  src: "17.png"},
        {class: "18",  src: "18.png"},
        {class: "18",  src: "18.png"}
      ]
    }
  },
  created(){
    for(let i = this.cards.length-1; i > 0; i--) {
      let randomIndex = Math.floor(Math.random() * i);
      let card = this.cards[i];
      this.cards[i] = this.cards[randomIndex];
      this.cards[randomIndex] = card;
    } 
  },
  methods:{
    getImgUrl(img) {
      return require("./assets/" + img);
    },
    showCard(card){
      if ((this.ClickedCards == 0) && (this.GameStage == 1)){
        card.clicked = !card.clicked;
        this.ClickedCards += 1; 
        this.prevCard = card;
        this.prevCardClass = card.class;
      } else {
      if ((this.ClickedCards == 1) && (this.GameStage == 1)){
        this.ClickedCards += 1; 
        card.clicked = !card.clicked;
        this.turns += 1;
        if (card.class == this.prevCardClass){
            setTimeout(()=>{
              this.FoundedCards += 1;
              this.prevCard.found = 1;
              card.found = !card.found;
            },600);
          } else{
            setTimeout(()=>{
              this.prevCard.clicked = 0;
              card.clicked = !card.clicked;
            },600);
          }
          setTimeout(()=>{
            this.ClickedCards = 0; 
            this.prevCard = null;
            this.prervCardClass = "";
          },600);
        } 
      }
    },
    startGame(){
      if (this.GameStage == 0){
        this.GameStage = 1;
        let sec = 0;
        let timer = setInterval(()=>{
          if (this.FoundedCards == 18){
            clearInterval(timer);
          } else {  
            if (sec == 59){
              this.timerSec = "00";
              sec = 0;
              this.timerMin += 1;
            } else{
              sec += 1; 
              if ( sec < 10){
              this.timerSec = "0" + sec;
              } else {
                this.timerSec = sec;
              }
            }
          }
        },1000);
      }
    }
  }
};
</script>

<style>
#app{
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  background-image: url(./assets/background.jpg);
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
body { 
  margin: 0;
}
.cards{
  display: flex;
  flex-wrap: wrap;
}
.card-container {
	position: relative;
	width: 16.6%;
  height: 150px;
  margin: auto;
}
.front{
  display: flex;
	position: absolute;
	left: 25%; right: 0; top: 0; bottom: 0;
}
.front.clicked{
  display: none;
}
.front.clicked.found{
  display: none;
}
.back{
  display: none;
	position: absolute;
	left: 25%; right: 0; top: 0; bottom: 0;
}
.back.clicked{
  display: flex;
}
.back.clicked.found{
  display: none;
}
.stats{
  color:rgb(210, 185, 90);
  font-size: 20px;
  font-weight: 700;
  display: inline;
}
.start{
  color: rgb(210, 185, 100);
  font-size: 20px;
  font-weight: 700;
  background-color: rgba(115, 144, 195, 0.55);
  border-color: rgb(210, 185, 100);
  border-radius: 99px;
  border-width: 3px;
  outline: none;
}
</style>
