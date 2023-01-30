<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(750 - 16 * 4) / Math.sqrt(cardContent.length) - 16}px`,
      width: `${
        (((820 - 16 * 4) / Math.sqrt(cardContent.length) - 16) * 3) / 4
      }px`,
      perspective: `${
        ((((820 - 16 * 4) / Math.sqrt(cardContent.length) - 16) * 3) / 4) * 2
      }px`,
    }"
  >
    <div
      class="crad_inner"
      :class="{ isflipped: isflipped }"
      @click="onToggleFlipCard"
    >
      <div class="card_face card_face--front">
        <div
          class="card_content"
          :style="{
            backgroundSize: `${
              (((820 - 16 * 4) / Math.sqrt(cardContent.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((820 - 16 * 4) / Math.sqrt(cardContent.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card_face card_face--back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    cardContent: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isflipped: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      this.isflipped = !this.isflipped;
      if (this.isflipped) this.$emit("onFlip", this.card);
    },
    onFlipBackCard() {
      this.isflipped = false;
    },
    onEnableDisableModle() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 8px;
  margin-bottom: 8px;
}

.crad_inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card.disabled .crad_inner {
  cursor: default;
}

.crad_inner.isflipped {
  transform: rotateY(-180deg);
}

.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card_face--front .card_content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  height: 100%;
  width: 100%;
}

.card_face--back {
  transform: rotateY(-180deg);
}

.card_face--back .card_content {
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
}
</style>
