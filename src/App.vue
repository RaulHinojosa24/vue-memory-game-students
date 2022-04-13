<script setup>
// Importamos el JSON. Vue ya lo convierte en un array de objetos y lo poe en la variable pokedex

import pokedex from "./assets/pokedex.json";
import backCardImage from './assets/back-card.png';
import ChessBoard from "./components/ChessBoard.vue";
import { reactive } from "@vue/reactivity";

let cards = pokedex.slice(0, 10);

cards = cards.map((pokemon, index) => {
  return {
    uid: index,
    id: pokemon.id,
    imageUrl: `/pokemons/${pokemon.id.toString().padStart(3, '0')}.png`,
    name: pokemon.name.english,
    isRevealed: false
  }
});

const state = reactive({
  score: 0
});

function icreaseScoreBy(amount) {
  state.score += amount;
}

</script>

<template>
  <header>
    <h1>¡PokeMemory!</h1>
    <h2>Score: {{ state.score }}</h2>
  </header>

  <main id="main">
    <ChessBoard :cards="cards" :backCard="backCardImage" @increaseScore="(x) => icreaseScoreBy(x)" />
  </main>
</template>

<style>
@import "./assets/base.css";

#app,
#main {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.chess-board {
  width: 100%;
}

@media (min-width: 1024px) {
  .chess-board {
    width: 70%;
  }
}
</style>
