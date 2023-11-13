<script setup>
import { ref, watchEffect, computed } from "vue";
import { sample } from "lodash";
import _ from "lodash";
const userChoice = ref();
const computerChoice = ref();
const userScore = ref(0);
const computerScore = ref(0);
const rocks = ref("rocks");
const scissors = ref("scissors");
const paper = ref("paper");
const counter = ref(0);

const choose = (pick) => {
  userChoice.value = pick;

  // Use lodash to randomly select the computer's choice
  const choices = [rocks.value, paper.value, scissors.value];
  computerChoice.value = _.sample(choices);

  // Determine the winner
  if (userChoice.value === computerChoice.value) {
    // It's a tie
  } else if (
    (userChoice.value === rocks.value &&
      computerChoice.value === scissors.value) ||
    (userChoice.value === paper.value &&
      computerChoice.value === rocks.value) ||
    (userChoice.value === scissors.value &&
      computerChoice.value === paper.value)
  ) {
    // User wins
    userScore.value++;
  } else {
    // Computer wins
    computerScore.value++;
  }
};

// timer
watchEffect(() => {
  if (counter.value >= 0) {
    setTimeout(() => {
      counter.value++;
    }, 1000);
  }
});

const hours = computed(() => Math.floor(counter.value / 60 / 60));
const minutes = computed(() => Math.floor((counter.value / 60) % 60));
const seconds = computed(() => Math.floor(counter.value % 60));
</script>

<template>
  <div class="game-container">
    <h1 class="game-title">Rock Paper Scissors</h1>
    <div class="button-container">
      <button @click="choose('rocks')">Rocks</button>
      <button @click="choose('paper')">Paper</button>
      <button @click="choose('scissors')">Scissors</button>
    </div>
    <h1 class="score">User({{ userScore }}) - Computer({{ computerScore }})</h1>
    <p class="red-color" v-show="userChoice === computerChoice">
      {{ userChoice }} vs {{ computerChoice }}
    </p>
    <p class="timer">{{ hours }} : {{ minutes }} : {{ seconds }}</p>
  </div>
</template>

<style scoped>
.game-container {
  text-align: center;
  margin-top: 150px;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.game-title {
  color: cadetblue;
  font-weight: bold;
}

.button-container {
  justify-content: space-between;
  width: 100%;
  margin-top: 10px;
  margin: 5px 0;
}

button {
  padding: 0.8rem;
  margin: 20px 2px;
}

.score {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 15px;
}

.timer {
  font-size: 25px;
  font-weight: bold;
  width: 150px;
  background-color: thistle;
  text-align: center;
}

.red-color {
  color: #990909;
  font-size: 20px;
}
</style>
