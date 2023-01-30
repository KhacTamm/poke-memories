<template>
  <div class="screen">
    <div
      class="screen_inner"
      :style="{
        width: `${
          ((((820 - 16 * 4) / Math.sqrt(cardContent.length) - 16) * 3) / 4 +
            16) *
          Math.sqrt(cardContent.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContent"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule($event)"
        :cardContent="cardContent"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";
export default {
  props: {
    cardContent: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        this.$refs[`card-${this.rules[0].index}`][0].onEnableDisableModle();
        this.$refs[`card-${this.rules[1].index}`][0].onEnableDisableModle();
        this.rules = [];

        const disableElements = document.querySelectorAll(
          ".screen .card.disabled"
        );

        if (
          disableElements &&
          disableElements.length === this.cardContent.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinish");
          }, 920);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          this.rules = [];
        }, 800);
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: #ffffff;
  color: var(--light);
}

.screen_inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>
