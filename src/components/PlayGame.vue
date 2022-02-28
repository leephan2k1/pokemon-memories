<template>
  <div class="screen">
    <CardGame
      v-for="(card, index) in cardsContext"
      :key="index"
      :imageBackFaceUrl="`images/${card}.png`"
      :ref="`card-${index}`"
      :card="{ index, value: card }"
      @onFlip="checkRule($event)"
    />
  </div>
</template>

<script>
import CardGame from "./Card.vue";

export default {
  data() {
    return {
      rules: [],
    };
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardGame,
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        //add class disabled
        this.$refs[`card-${this.rules[0].index}`][0].onDisabledFlip();
        this.$refs[`card-${this.rules[1].index}`][0].onDisabledFlip();

        //reset rules
        this.rules = [];

        //check result
        const disabledElements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardsContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinished");
          }, 900);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          //close 2 cards
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          //reset rules
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>
