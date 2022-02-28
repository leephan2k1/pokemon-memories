<template>
  <StartGame
    v-if="statusMatch === 'default'"
    @onStart="handleSelectModes($event)"
  />
  <PlayGame
    v-if="statusMatch === 'play'"
    :cardsContext="settings.cardsContext"
  />
</template>

<script>
import StartGame from "./components/StartGame.vue";
import PlayGame from "./components/PlayGame.vue";
import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: {
    StartGame,
    PlayGame,
  },
  data() {
    return {
      statusMatch: "default",
      settings: {
        totalCards: 0,
        cardsContext: [],
        startedAt: null,
      },
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
      this.settings.createdAt = new Date().getTime();

      this.statusMatch = "play";
    },
  },
};
</script>

<style lang="scss"></style>
