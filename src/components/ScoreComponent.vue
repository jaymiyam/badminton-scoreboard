<template>
  <div class="score-area">
    <div class="score" :class="{ 'game-point': isMatchPoint }">
      {{ half.score }}
    </div>
    <div class="score-controls">
      <button class="btn-score-controls" @click="changeScore(-1)">-</button>
      <button class="btn-score-controls" @click="changeScore(1)">+</button>
    </div>
  </div>
</template>

<script setup>
const { half, isMatchPoint } = defineProps({
  half: {
    type: Object,
    required: true,
  },
  isMatchPoint: {
    type: Boolean,
    required: false,
  },
});

const emits = defineEmits(['changeScore']);

function changeScore(incrementor) {
  emits('changeScore', { incrementor: incrementor, name: half.name });
}
</script>

<style scoped>
/* score area */
.score-area {
  display: grid;
  gap: 2px;
  background-color: white;
}
.score {
  color: var(--clr-neutral-700);
  font-size: 6rem;
  font-weight: 700;

  display: flex;
  justify-content: center;
  align-items: center;
}

.score.game-point {
  color: var(--clr-red);
}
.score-controls {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1rem;
  place-items: center;
  background-color: white;
}
.btn-score-controls {
  width: 75px;
  height: 75px;
  aspect-ratio: 1 / 1;
  background-color: var(--clr-orange);
  font-size: 2rem;
  border-radius: 50%;
  display: grid;
  display: flex;
  justify-content: center;
  align-items: center;
}

.btn-score-controls:active {
  background-color: var(--clr-orange-bg);
}

@media (max-width: 900px) {
  .score {
    font-size: 4.5rem;
  }
}
</style>
