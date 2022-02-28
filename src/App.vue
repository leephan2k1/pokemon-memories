<template>
  <StartGame
    v-if="statusMatch === 'default'"
    @onStart="handleSelectModes($event)"
  />
  <PlayGame
    v-if="statusMatch === 'play'"
    :cardsContext="settings.cardsContext"
    @onFinished="onGetResult"
  />
  <ResultGame
    v-if="statusMatch === 'result'"
    :timer="timer"
    @startAgain="onStartAgain"
  />
  <FooterGame />
</template>

<script>
import StartGame from "./components/StartGame.vue";
import PlayGame from "./components/PlayGame.vue";
import ResultGame from "./components/ResultGame.vue";
import FooterGame from "./components/FooterGame.vue";
import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: {
    StartGame,
    PlayGame,
    ResultGame,
    FooterGame,
  },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalCards: 0,
        cardsContext: [],
        startedAt: null,
      },
      timer: 0,
    };
  },
  methods: {
    handleSelectModes(config) {
      this.statusMatch = "play";
      this.settings.totalCards = config.totalCards;
      const firstArrayCards = Array.from(
        { length: config.totalCards / 2 },
        (v, i) => i + 1
      );
      const secondArrayCards = [...firstArrayCards];
      const arrayCards = [...firstArrayCards, ...secondArrayCards];
      this.settings.cardsContext = shuffled(shuffled(shuffled(arrayCards)));
      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "play";
    },
    onGetResult() {
      //get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch screen
      this.statusMatch = "result";
    },
    onStartAgain() {
      this.statusMatch = "default";
    },
  },
};
</script>

<style lang="scss"></style>
