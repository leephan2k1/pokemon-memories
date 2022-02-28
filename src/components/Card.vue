<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(780 - 16 * 4) / Math.sqrt(cardsContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipped"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imageBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardGame",
  props: {
    imageBackFaceUrl: {
      type: String,
      require: true,
    },
    flipping: {
      type: Boolean,
      default: false,
    },
    card: {
      type: Object,
    },
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
    };
  },
  methods: {
    onToggleFlipped() {
      if (this.isDisabled) return false;
      //check 2 cards are flipping
      if (this.flipping) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) {
        this.$emit("onFlip", this.card);
      }
    },
    onFlipBackCard() {
      this.isFlipped = false;
    },
    onDisabledFlip() {
      this.isDisabled = true;
    },
  },
};
</script>

<style scoped lang="scss">
.card.disabled {
  .card__inner {
    cursor: default;
  }
}
.card {
  display: inline-block;
  margin: 0 1rem 1rem 0;
  width: 90px;
  height: 120px;
  perspective: 150px;
  &__inner {
    width: 100%;
    height: 100%;
    transition: all 1s;
    transform-style: preserve-3d;
    cursor: pointer;
    position: relative;
    .card__face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      overflow: hidden;
      border-radius: 1rem;
      padding: 1rem;
      box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
      &--back {
        background-color: var(--light);
        transform: rotateY(-180deg);
        .card__content {
          background-position: center center;
          background-repeat: no-repeat;
          background-size: contain;
          height: 100%;
          width: 100%;
        }
      }
      &--front {
        .card__content {
          background: url("../assets/images/icon_back.png") no-repeat center
            center;
          width: 100%;
          height: 100%;
        }
      }
    }
  }
  &__inner.is-flipped {
    transform: rotateY(-180deg);
  }
}
</style>
