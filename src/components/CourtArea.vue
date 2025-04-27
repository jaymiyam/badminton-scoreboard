<template>
  <div class="court-area">
    <div class="half-court right-border">
      <div class="side">
        <div class="player">
          <span class="player-name">{{ getPlayerName(true, true) }}</span
          ><sup
            class="service"
            :class="{ active: serviceHalf === 'left' && serviceIndicator }"
          >
            🔴</sup
          >
        </div>
      </div>
      <div class="side">
        <div class="player">
          <span class="player-name">{{ getPlayerName(true, false) }}</span
          ><sup
            class="service"
            :class="{ active: serviceHalf === 'left' && !serviceIndicator }"
          >
            🔴</sup
          >
        </div>
      </div>
    </div>
    <div class="half-court left-border">
      <div class="side">
        <div class="player">
          <span class="player-name">{{ getPlayerName(false, false) }}</span
          ><sup
            class="service"
            :class="{ active: serviceHalf === 'right' && !serviceIndicator }"
          >
            🔴</sup
          >
        </div>
      </div>
      <div class="side">
        <div class="player">
          <span class="player-name">{{ getPlayerName(false, true) }}</span
          ><sup
            class="service"
            :class="{ active: serviceHalf === 'right' && serviceIndicator }"
          >
            🔴</sup
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const { players, serviceIndicator, serviceHalf } = defineProps({
  players: {
    type: Array,
    required: true,
  },
  serviceIndicator: {
    type: Number,
    required: true,
    default: 0,
  },
  serviceHalf: {
    type: String,
    required: true,
  },
});

function getPlayerName(isLeftHalf, isLeftSide) {
  return players.find(
    (player) =>
      player.isLeftHalf === isLeftHalf && player.isLeftSide === isLeftSide
  ).name;
}
</script>

<style scoped>
/* court area */
.court-area {
  height: 250px;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 2px;
  background-color: white;
  border-radius: 0.5rem;
  overflow: hidden;
}

.half-court {
  width: 100%;
  height: 100%;
  display: grid;
  grid-template-rows: repeat(2, 1fr);
  gap: 2px;
  background-color: white;
}

.right-border {
  border-right: 2px solid white;
}

.left-border {
  border-left: 2px solid white;
}

.side {
  background-color: var(--clr-green);
  display: flex;
  justify-content: center;
  align-items: center;
}

.player-name {
  font-size: 1.25rem;
  font-weight: 500;
}

.service {
  display: none;
}

.service.active {
  display: inline;
}

@media (max-width: 900px) {
  .court-area {
    height: 220px;
  }
  .side {
    padding: 0.5rem;
  }
  .player-name {
    font-size: 1.2rem;
  }
}
</style>
