<template>
  <div>
    <div class="container">
      <NewGameSingles
        v-if="toggleNewGameSingles"
        @submit-players="handleSubmitPlayers"
        @cancel-submit="handleCancelSubmit"
      />
      <NewGameDoubles
        v-else-if="toggleNewGameDoubles"
        @submit-players="handleSubmitPlayers"
        @cancel-submit="handleCancelSubmit"
      />
      <div v-else class="game-wrapper">
        <ScoreComponent
          :half="leftHalf"
          :isMatchPoint="isMatchPoint === 'left'"
          @change-score="handleChangeScore"
        />
        <CourtArea
          :players="players"
          :service-indicator="serviceIndicator"
          :service-half="serviceHalf"
        />
        <ScoreComponent
          :half="rightHalf"
          :isMatchPoint="isMatchPoint === 'right'"
          @change-score="handleChangeScore"
        />
      </div>
      <MainControls
        v-if="!toggleNewGameDoubles && !toggleNewGameSingles"
        @new-game-singles="onNewGameSingles"
        @new-game-doubles="onNewGameDoubles"
        @reset-game="onResetGame"
        @open-rules-modal="onOpenRulesModal"
      />
    </div>
  </div>
  <RulesModal v-if="toggleRulesModal" @close-modal="handleCloseModal" />
</template>

<script setup>
import { ref, computed } from 'vue';
import ScoreComponent from './components/ScoreComponent.vue';
import CourtArea from './components/CourtArea.vue';
import NewGameSingles from './components/NewGameSingles.vue';
import NewGameDoubles from './components/NewGameDoubles.vue';
import MainControls from './components/MainControls.vue';
import RulesModal from './components/RulesModal.vue';
import ScoreChart from './components/ScoreChart.vue';

// states
const gameMode = ref('doubles');
const serviceHalf = ref('left');
const serviceIndicator = ref(0);
const prevServiceHalf = ref(null);
const prevServiceIndicator = ref(null);
const scoringHistory = ref([[0], [0]]);
const leftHalf = ref({
  name: 'left',
  score: 0,
});
const rightHalf = ref({
  name: 'right',
  score: 0,
});
const players = ref([
  {
    name: 'player1',
    isLeftHalf: true,
    isLeftSide: true,
  },
  {
    name: 'player2',
    isLeftHalf: true,
    isLeftSide: false,
  },
  {
    name: 'player3',
    isLeftHalf: false,
    isLeftSide: true,
  },
  {
    name: 'player4',
    isLeftHalf: false,
    isLeftSide: false,
  },
]);
const toggleNewGameSingles = ref(false);
const toggleNewGameDoubles = ref(false);
const toggleRulesModal = ref(false);

const isDeuce = computed(() => {
  return leftHalf.value.score >= 20 && rightHalf.value.score >= 20;
});

const isMatchPoint = computed(() => {
  if (isDeuce.value) {
    if (
      leftHalf.value.score - rightHalf.value.score >= 1 ||
      leftHalf.value.score >= 29
    ) {
      return 'left';
    } else if (
      rightHalf.value.score - leftHalf.value.score >= 1 ||
      rightHalf.value.score >= 29
    ) {
      return 'right';
    }
  } else {
    if (leftHalf.value.score >= 20) {
      return 'left';
    } else if (rightHalf.value.score >= 20) {
      return 'right';
    }
  }
});

// functions

function handleChangeScore(payload) {
  if (payload.name === 'left') {
    // 1. update score
    leftHalf.value.score += payload.incrementor * 1;
    if (leftHalf.value.score <= 0) {
      leftHalf.value.score = 0;
    }

    // 2. update service state
    if (payload.incrementor > 0) {
      serviceHalf.value = 'left';
      scoringHistory.value[0].push(leftHalf.value.score);
      scoringHistory.value[1].push(rightHalf.value.score);
    } else {
      scoringHistory.value[0].pop();
      scoringHistory.value[1].pop();
    }
  } else if (payload.name === 'right') {
    // 1. update score
    rightHalf.value.score += payload.incrementor * 1;
    if (rightHalf.value.score <= 0) {
      rightHalf.value.score = 0;
    }

    // 2. update service right
    if (payload.incrementor > 0) {
      serviceHalf.value = 'right';
      scoringHistory.value[0].push(leftHalf.value.score);
      scoringHistory.value[1].push(rightHalf.value.score);
    } else {
      scoringHistory.value[0].pop();
      scoringHistory.value[1].pop();
    }
  }

  // update service position
  updateServicePosition(serviceHalf.value);

  // update player positions
  if (gameMode.value === 'singles' && payload.incrementor > 0) {
    switchPlayersPositionSingles();
  }
  if (gameMode.value === 'doubles' && payload.incrementor > 0) {
    switchPlayersPositionDoubles();
  }

  prevServiceHalf.value = serviceHalf.value;
  prevServiceIndicator.value = serviceIndicator.value;

  // check for win
  checkForWin();
}

function switchPlayersPositionSingles() {
  if (serviceHalf.value === prevServiceHalf.value) {
    players.value.forEach((player) => (player.isLeftSide = !player.isLeftSide));
  } else if (
    serviceHalf.value != prevServiceHalf.value &&
    serviceIndicator.value != prevServiceIndicator.value
  ) {
    players.value.forEach((player) => (player.isLeftSide = !player.isLeftSide));
  }
}

function switchPlayersPositionDoubles() {
  if (
    serviceHalf.value === 'left' &&
    serviceHalf.value === prevServiceHalf.value
  ) {
    players.value
      .filter((player) => player.isLeftHalf)
      .forEach((player) => (player.isLeftSide = !player.isLeftSide));
  } else if (
    serviceHalf.value === 'right' &&
    serviceHalf.value === prevServiceHalf.value
  ) {
    players.value
      .filter((player) => !player.isLeftHalf)
      .forEach((player) => (player.isLeftSide = !player.isLeftSide));
  }
}

function updateServicePosition(half) {
  if (half === 'left') {
    serviceIndicator.value = leftHalf.value.score % 2;
  } else if (half === 'right') {
    serviceIndicator.value = rightHalf.value.score % 2;
  }
}

function checkForWin() {
  if (isDeuce.value) {
    if (
      Math.abs(leftHalf.value.score - rightHalf.value.score) >= 2 ||
      leftHalf.value.score >= 30 ||
      rightHalf.value.score >= 30
    ) {
      alert(
        `Game won by ${serviceHalf.value} team! Final score: ${leftHalf.value.score} : ${rightHalf.value.score}`
      );
    }
  } else {
    if (leftHalf.value.score >= 21 || rightHalf.value.score >= 21) {
      alert(
        `Game won by ${serviceHalf.value} team! Final score: ${leftHalf.value.score} : ${rightHalf.value.score}`
      );
    }
  }
}

// game setup functions

function handleSubmitPlayers([leftHalfPlayers, rightHalfPlayers, newGameMode]) {
  onResetGame();

  gameMode.value = newGameMode;

  players.value[0].name = leftHalfPlayers.find(
    (player) => !player.initiation
  ).name;
  players.value[1].name = leftHalfPlayers.find(
    (player) => player.initiation
  ).name;
  players.value[2].name = rightHalfPlayers.find(
    (player) => !player.initiation
  ).name;
  players.value[3].name = rightHalfPlayers.find(
    (player) => player.initiation
  ).name;

  let leftInitiation = leftHalfPlayers.find(
    (player) => player.initiation
  ).initiation;

  if (leftInitiation === 'serve') {
    serviceHalf.value = 'left';
  } else if (leftInitiation === 'receive') {
    serviceHalf.value = 'right';
  }
}

function handleCancelSubmit() {
  toggleNewGameDoubles.value = false;
  toggleNewGameSingles.value = false;
}

// main controls handling

function onNewGameSingles() {
  toggleNewGameDoubles.value = false;
  toggleNewGameSingles.value = true;
}

function onNewGameDoubles() {
  toggleNewGameSingles.value = false;
  toggleNewGameDoubles.value = true;
}

function onOpenRulesModal() {
  toggleRulesModal.value = true;
}

function handleCloseModal() {
  toggleRulesModal.value = false;
}

// reset game
function onResetGame() {
  gameMode.value = 'doubles';
  toggleNewGameSingles.value = false;
  toggleNewGameDoubles.value = false;

  leftHalf.value = {
    name: 'left',
    score: 0,
  };

  rightHalf.value = {
    name: 'right',
    score: 0,
  };

  serviceHalf.value = 'left';
  serviceIndicator.value = 0;

  players.value = [
    {
      name: 'player1',
      isLeftHalf: true,
      isLeftSide: true,
    },
    {
      name: 'player2',
      isLeftHalf: true,
      isLeftSide: false,
    },
    {
      name: 'player3',
      isLeftHalf: false,
      isLeftSide: true,
    },
    {
      name: 'player4',
      isLeftHalf: false,
      isLeftSide: false,
    },
  ];
}
</script>

<style scoped>
.container {
  max-width: 900px;
  display: grid;
  gap: 2rem;
}

.game-wrapper {
  display: grid;
  grid-template-columns: 1fr 2fr 1fr;
  gap: 2rem;
}

@media (max-width: 900px) {
  .game-wrapper {
    gap: 1rem;
  }
  .score {
    font-size: 4.5rem;
  }
  .court-area {
    height: 220px;
  }
  .side {
    padding: 0.5rem;
  }
  .player-name {
    font-size: 1.2rem;
  }
  .btn-main-controls {
    padding-block: 1rem;
  }
}
</style>
