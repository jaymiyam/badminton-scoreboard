<template>
  <Teleport to="body">
    <div class="modal-overlay">
      <div class="modal-content">
        <button
          type="button"
          class="btn-close-modal"
          @click="$emit('closeModal')"
        >
          x
        </button>
        <h2 class="accent-color">Badminton Match Basic Rules</h2>

        <ul>
          <li>A match consists of the best of three games of 21 points.</li>
          <li>The player/pair winning a rally adds a point to its score.</li>
          <li>
            At 20-all, the player/pair which first gains a 2-point lead wins
            that game.
          </li>
          <li>At 29-all, the side scoring the 30th point wins that game.</li>
          <li>The player/pair winning a game serves first in the next game.</li>
          <li>
            A legal serve must be hit diagonally over the net and across the
            court.
          </li>
          <li>A player is not able to hit the shuttlecock twice.</li>
        </ul>
      </div>
    </div>
  </Teleport>
</template>

<script setup></script>

<style scoped>
.modal-overlay {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);

  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content h2 {
  text-align: center;
}

.modal-content li {
  font-size: 1.125rem;
  margin-bottom: 0.25rem;
}
.modal-content {
  background-color: white;
  color: #1a1a1a;
  padding: 2rem;
  border-radius: 2rem;
  position: absolute;
}

.btn-close-modal {
  position: absolute;
  top: 1rem;
  right: 1rem;
  width: 40px;
  height: 40px;
  font-size: 1.5rem;
  padding: 0;
  background-color: transparent;
  color: #1a1a1a;
}
</style>
