<template>
  <div class="form-wrapper">
    <h2 class="accent-color">New Game (Doubles)</h2>
    <form>
      <div class="input-area">
        <div>
          <h3>Left side</h3>
          <div class="input-group">
            <label for="player-1">Player 1: </label>
            <input
              type="text"
              name="player-1"
              id="player-1"
              v-model.trim="leftHalfPlayers[0].name"
            />
          </div>
          <div class="radio-group">
            <label for="player-1-serve">
              <input
                type="radio"
                name="left-serve-receive"
                id="player-1-serve"
                value="serve"
                @change="(e) => setInitiation(e, 'left', 0)"
              />
              Serve</label
            >
            <label for="player-1-receive">
              <input
                type="radio"
                name="left-serve-receive"
                id="player-1-receive"
                value="receive"
                @change="(e) => setInitiation(e, 'left', 0)"
              />
              Receive</label
            >
          </div>
          <div class="input-group">
            <label for="player-2">Player 2: </label>
            <input
              type="text"
              name="player-2"
              id="player-2"
              v-model.trim="leftHalfPlayers[1].name"
            />
          </div>
          <div class="radio-group">
            <label for="player-2-serve">
              <input
                type="radio"
                name="left-serve-receive"
                id="player-2-serve"
                value="serve"
                @change="(e) => setInitiation(e, 'left', 1)"
              />
              Serve</label
            >
            <label for="player-2-receive">
              <input
                type="radio"
                name="left-serve-receive"
                id="player-2-receive"
                value="receive"
                @change="(e) => setInitiation(e, 'left', 1)"
              />
              Receive</label
            >
          </div>
        </div>
        <div>
          <h3>Right side</h3>
          <div class="input-group">
            <label for="player-3">Player 3: </label>
            <input
              type="text"
              name="player-3"
              id="player-3"
              v-model.trim="rightHalfPlayers[0].name"
            />
          </div>
          <div class="radio-group">
            <label for="player-3-serve">
              <input
                type="radio"
                name="right-serve-receive"
                id="player-3-serve"
                value="serve"
                @change="(e) => setInitiation(e, 'right', 0)"
              />
              Serve</label
            >
            <label for="player-3-receive">
              <input
                type="radio"
                name="right-serve-receive"
                id="player-3-receive"
                value="receive"
                @change="(e) => setInitiation(e, 'right', 0)"
              />
              Receive</label
            >
          </div>
          <div class="input-group">
            <label for="player-4">Player 4: </label>
            <input
              type="text"
              name="player-4"
              id="player-4"
              v-model.trim="rightHalfPlayers[1].name"
            />
          </div>
          <div class="radio-group">
            <label for="player-4-serve">
              <input
                type="radio"
                name="right-serve-receive"
                id="player-4-serve"
                value="serve"
                @change="(e) => setInitiation(e, 'right', 1)"
              />
              Serve</label
            >
            <label for="player-4-receive">
              <input
                type="radio"
                name="right-serve-receive"
                id="player-4-receive"
                value="receive"
                @change="(e) => setInitiation(e, 'right', 1)"
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

function setInitiation(e, half, index) {
  if (half === 'left') {
    leftHalfPlayers.value.forEach((player) => {
      player.initiation = null;
    });

    leftHalfPlayers.value[index].initiation = e.target.value;
  } else if (half === 'right') {
    rightHalfPlayers.value.forEach((player) => {
      player.initiation = null;
    });
    rightHalfPlayers.value[index].initiation = e.target.value;
  }
}

function submitPlayers() {
  if (
    leftHalfPlayers.value.some((player) => !player.name) ||
    rightHalfPlayers.value.some((player) => !player.name)
  ) {
    alert('Error: Please enter all player names');
    return;
  } else if (
    leftHalfPlayers.value.every((player) => !player.initiation) ||
    rightHalfPlayers.value.every((player) => !player.initiation)
  ) {
    alert('Error: Please select initial serve and receive');
    return;
  } else if (
    leftHalfPlayers.value.some((player) => player.initiation === 'serve') &&
    rightHalfPlayers.value.some((player) => player.initiation === 'serve')
  ) {
    alert('Error: Please select no more than one initial server.');
    return;
  } else if (
    leftHalfPlayers.value.some((player) => player.initiation === 'receive') &&
    rightHalfPlayers.value.some((player) => player.initiation === 'receive')
  ) {
    alert('Error: Please select no more than one initial receiver.');
    return;
  }

  emits('submitPlayers', [
    leftHalfPlayers.value,
    rightHalfPlayers.value,
    'doubles',
  ]);
}

function cancelSubmit() {
  emits('cancelSubmit');
}
</script>
