<template>
  <div class="main-controls">
    <button class="btn-main-controls" @click="$emit('newGameSingles')">
      New Singles Match
    </button>
    <button class="btn-main-controls" @click="$emit('newGameDoubles')">
      New Doubles Match
    </button>
    <button class="btn-main-controls" @click="handleResetGame">
      Reset Game
    </button>
    <button class="btn-main-controls" @click="$emit('openRulesModal')">
      Basic Rules
    </button>
  </div>
</template>

<script setup>
const emits = defineEmits([
  'newGameSingles',
  'newGameDoubles',
  'resetGame',
  'openRulesModal',
]);
function handleResetGame() {
  let text = 'Do you wish to reset the current game?';
  if (confirm(text)) {
    emits('resetGame');
  } else {
    return;
  }
}
</script>

<style scoped>
/* main controls */
.main-controls {
  grid-column: 1 / -1;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 1rem;
  background-color: white;
}
.btn-main-controls {
  padding-block: 0.75rem;
  border-radius: 100px;
  line-height: 1.5;
}

@media (max-width: 900px) {
  .btn-main-controls {
    padding-block: 1rem;
  }
}
</style>
