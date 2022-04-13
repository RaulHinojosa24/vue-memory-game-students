<!-- ChessBoard Component -->
<script setup>
import { computed, reactive, ref } from "vue";
import Card from "./Card.vue";

const props = defineProps({
    cards: Array,
    backCard: String
});

const emit = defineEmits(['increaseScore']);

const canPlay = ref(true);
const revealedCards = [];

const allCards = ref(JSON.parse(JSON.stringify(props.cards.concat(props.cards))));

allCards.value.sort(() => Math.random() - 0.5);

function cardClicked(card) {
    if (canPlay.value) {
        if (card.isRevealed == true) return;

        flipCard(card);
        revealedCards.push(card);
        checkRevealedCards();
    }
}

function checkRevealedCards() {
    if (revealedCards.length == 2) {
        canPlay.value = false;

        const areEqual = revealedCards[0].id == revealedCards[1].id;

        if (areEqual) {
            emit('increaseScore', 10);

            revealedCards.pop()
            revealedCards.pop()

            canPlay.value = true;
            return;
        }

        setTimeout(() => {
            revealedCards.forEach(c => flipCard(c));

            revealedCards.pop()
            revealedCards.pop()

            canPlay.value = true;
        }, 2000);
    }
}

function flipCard(card) {
    card.isRevealed = !card.isRevealed;
}

const mixedCards = reactive(allCards);

const gameCompleted = computed(() => {
    return allCards.value.every(c => c.isRevealed == true)
});

</script>

<template>
    <div class="chess-board" v-if="!gameCompleted">
        <Card @click="cardClicked(card)" class="card" v-for="card in mixedCards" :back="props.backCard" :key="card.uid"
            :card="card" />
    </div>
    <div v-else>
        <h1>Game completed!!</h1>
    </div>
</template>

<style>
@import "../assets/base.css";

.chess-board {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    gap: 20px;
    justify-content: center;
}

.card {
    display: flex;
    align-items: center;

    width: 100px;
    min-width: 100px;
    height: 142px;
    min-height: 142px;

    user-select: none;
}

.card img {
    min-width: 100%;
}
</style>