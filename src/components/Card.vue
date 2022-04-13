<!-- Card Component -->
<script setup>

const props = defineProps({
    back: String,
    card: Object
});

</script>

<template>
    <div class="card">
        <img v-if="card.isRevealed" :src="card.imageUrl" alt="">
        <img v-else :src="back" alt="">
    </div>
</template>

<style>
</style>