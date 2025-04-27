<template>
  <div class="form-wrapper">
    <h2 class="accent-color">New Game (Singles)</h2>
    <form>
      <div class="input-area">
        <div>
          <h3>Left side</h3>
          <div class="input-group">
            <label for="player-1">Player 1:</label>
            <input
              type="text"
              name="player-1"
              id="player-1"
              v-model="leftHalfPlayers[0].name"
            />
          </div>
          <div class="radio-group">
            <label for="player-1-serve">
              <input
                type="radio"
                name="left-serve-receive"
                id="player-1-serve"
                value="serve"
                v-model="leftHalfPlayers[0].initiation"
              />
              Serve</label
            >
            <label for="player-1-receive">
              <input
                type="radio"
                name="left-serve-receive"
                id="player-1-receive"
                value="receive"
                v-model="leftHalfPlayers[0].initiation"
              />
              Receive</label
            >
          </div>
        </div>
        <div>
          <h3>Right side</h3>

          <div class="input-group">
            <label for="player-2">Player 2:</label>
            <input
              type="text"
              name="player-2"
              id="player-2"
              v-model="rightHalfPlayers[0].name"
            />
          </div>
          <div class="radio-group">
            <label for="player-2-serve">
              <input
                type="radio"
                name="right-serve-receive"
                id="player-2-serve"
                value="serve"
                v-model="rightHalfPlayers[0].initiation"
              />
              Serve</label
            >
            <label for="player-2-receive">
              <input
                type="radio"
                name="right-serve-receive"
                id="player-2-receive"
                value="receive"
                v-model="rightHalfPlayers[0].initiation"
              />
              Receive</label
            >
          </div>
        </div>
      </div>
      <button class="btn-form" type="button" @click="submitPlayers">
        Start game
      </button>
      <button class="btn-form" type="button" @click="cancelSubmit">
        Go back
      </button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const emits = defineEmits(['submitPlayers', 'cancelSubmit']);

const leftHalfPlayers = ref([
  {
    name: '',
    initiation: null,
  },
  {
    name: '',
    initiation: null,
  },
]);

const rightHalfPlayers = ref([
  {
    name: '',
    initiation: null,
  },
  {
    name: '',
    initiation: null,
  },
]);

function submitPlayers() {
  if (!leftHalfPlayers.value[0].name || !rightHalfPlayers.value[0].name) {
    alert('Error: Please enter all player names');
    return;
  } else if (
    !leftHalfPlayers.value[0].initiation ||
    !rightHalfPlayers.value[0].initiation
  ) {
    alert('Error: Please select initial serve and receive');
    return;
  } else if (
    leftHalfPlayers.value[0].initiation === 'serve' &&
    rightHalfPlayers.value[0].initiation === 'serve'
  ) {
    alert('Error: Please select no more than one initial server.');
    return;
  } else if (
    leftHalfPlayers.value[0].initiation === 'receive' &&
    rightHalfPlayers.value[0].initiation === 'receive'
  ) {
    alert('Error: Please select no more than one initial receiver.');
    return;
  }

  emits('submitPlayers', [
    leftHalfPlayers.value,
    rightHalfPlayers.value,
    'singles',
  ]);
}

function cancelSubmit() {
  emits('cancelSubmit');
}
</script>
