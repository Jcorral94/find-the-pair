<template>
  <h2>Memory Game</h2>
  <div id="comment">{{ comments || "Select Card(s)" }}</div>
  <button v-if="endGame" @click="resetGame">Reset Game</button>
  <div id="attempts">Attempts: {{ attempts }}</div>
  <div id="cards">
    <Card
      v-for="(card, index) of cards"
      :key="card.name + index"
      :value="card.value"
      :name="card.name"
      :visible="card.visible"
      @click="flipCard(card, card.visible)"
    >
    </Card>
  </div>
</template>

<script setup>
import Card from "./Card.vue";
import availableCards from "../utils/availableCards";
import { ref } from "vue";

const cards = ref([...addCardPair()]);
const selectedCards = ref([]);
const comments = ref("");
const cardsMatched = ref([]);
const attempts = ref(0);
const endGame = ref(false);


function addCardPair() {
  //had an issue when creating the pair
  // i was creating an object that was referencing eachother and was modifying both objects to be visible
  //example:
  //const result = [{value: 1, index: 0}];
  //result.forEach(value=>{
  //  addedDupes.push(value);
  //  addedDupes.push(value);
  // })
  //addedDupes[0].value = 2
  //^ modified both objects

  let result = [];

  for (let i = 0; i < availableCards.length; i++) {
    const card = availableCards[i];
    result.push(Object.assign({}, card), Object.assign({}, card));
  }

  result = result.sort(() => 0.5 - Math.random());
  console.log(result);

  return result;
}

function flipCard(card, visible) {
  if (visible) return;

  if (selectedCards.value.length < 2) {
    card.visible = true;
    selectedCards.value.push(card);
  }
  //if we have 2 cards selected compare them
  if (selectedCards.value.length == 2) {
    attempts.value += 1;
    const one = selectedCards.value[0];
    const two = selectedCards.value[1];
    checkSelectedCards(one, two);
  }
}

function resetGame(){
  cards.value = [...addCardPair()];
  console.log(cards.value);
  selectedCards.value = [];
  comments.value = "";
  cardsMatched.value = [];
  attempts.value = 0;
  endGame.value = false;
}
function checkSelectedCards(one, two) {
  if (one.name == two.name) {
    comments.value = "Good job! That's correct 🫡";
    cardsMatched.value.push(one);
    if (cardsMatched.value.length == cards.value.length / 2) {
      console.log(
        cardsMatched.value,
        cards.value,
        "We have a completed board!"
      );
      comments.value = `You have completed the game! It took you ${attempts.value} attempts to find all pairs!`;
      endGame.value = true;

      return;
    }
  } else {
    //flip cards back over
    comments.value = "Those cards do not match! 👺";
    setTimeout(() => {
      one.visible = false;
      two.visible = false;
    }, 700);
  }

  setTimeout(() => (comments.value = ""), 700);
  selectedCards.value = [];
}
</script>

<style>
* {
  font-family: sans-serif;
}
#comment,
#attempts {
  padding: 10px;
}
#cards {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  width: 50vw;
}
</style>
