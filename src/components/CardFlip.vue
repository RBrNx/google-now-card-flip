<template>
  <div>
    <div id="cardFlipOverlay" :class="cardClass" @click="closeCardClone"></div>
    <div id="cardFlip" :style="customStyle" :class="cardClass">
      <div id="flipper" :style="{ transform: cardTransform }">
        <div id="cardFront">
          <component :is="cardFrontComponent" v-bind="{ itemData }"></component>
        </div>
        <div id="cardBack">
          <component :is="cardBackComponent"></component>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardClone",
  props: ["cardFrontComponent", "cardBackComponent"],
  data() {
    return {
      cardTransform: "rotateY(0deg)",
      customStyle: null,
      cardClass: null,
      clonedElement: null
    };
  },
  methods: {
    closeCardClone() {
      this.$el
        .querySelector("#cardFlip")
        .addEventListener("transitionend", this.onTransitionEnd);
      this.cardTransform = "rotateY(0deg)";
      document.body.classList.remove("overlayShown");

      const cardElement = this.clonedElement;
      const viewportOffset = cardElement.getBoundingClientRect();

      this.cardClass = null;

      this.customStyle = {
        height: `${cardElement.clientHeight}px`,
        width: `${cardElement.clientWidth}px`,
        left: `${viewportOffset.left}px`,
        top: `${viewportOffset.top}px`
      };
    },
    onTransitionEnd(e) {
      if (e.propertyName === "transform" && e.target.id === "cardFlip") {
        this.onClosed();
      }
    },
    onClosed() {
      this.$el.removeEventListener("transitionend", this.onTransitionEnd);
      this.clonedElement.style.opacity = 1;
      this.$el.style.opacity = 0;
      this.$emit("cardClosed");
    }
  },
  mounted() {
    const cardElement = document.getElementById(this.cardId);
    const viewportOffset = cardElement.getBoundingClientRect();
    this.clonedElement = cardElement;

    this.customStyle = {
      height: `${cardElement.clientHeight}px`,
      width: `${cardElement.clientWidth}px`,
      left: `${viewportOffset.left}px`,
      top: `${viewportOffset.top}px`
    };

    cardElement.style.opacity = 0;

    setTimeout(() => {
      const isOverHalfway =
        parseFloat(this.customStyle.left) +
          parseFloat(this.customStyle.width) / 2 >
        window.innerWidth / 2;
      const transform = isOverHalfway ? "rotateY(-180deg)" : "rotateY(180deg)";
      this.cardTransform = transform;
      this.cardClass = "shown";
      document.body.classList.add("overlayShown");
      this.$emit("cardOpened");
    }, 150);
  }
};
</script>

<style lang="scss">
@import "../global.scss";

.scrollContainer {
  height: 100%;
}

#cardFlipOverlay {
  width: 100vw;
  height: 100vh;
  position: fixed;
  left: 0;
  top: 0;
  background-color: #000;
  opacity: 0;
  transition: opacity 1.6s;

  &.shown {
    opacity: 0.7;
  }
}

#cardFlip {
  transition: 0.6s;
  background-color: transparent;
  position: fixed;
  perspective: 1000px;
  z-index: 100;

  &.shown {
    height: 100% !important;
    width: 100% !important;
    left: 0 !important;
    top: 0 !important;

    @include tablet {
      height: 90% !important;
      width: 80% !important;
      left: 50vw !important;
      top: 50vh !important;
      transform: translate(-50%, -50%) !important;
    }

    @include desktop {
      height: 80% !important;
      width: 50% !important;
      left: 50vw !important;
      top: 50vh !important;
      transform: translate(-50%, -50%) !important;
    }

    #cardBack {
      padding: 1px;
    }
  }

  #cardBack {
    padding: 15px;
    padding-top: 50px;
  }
}

#flipper {
  width: 100%;
  height: 100%;
  position: relative;
  -webkit-transition: 0.6s;
  transition: 0.6s;
  z-index: 99;
  transform-style: preserve-3d;
  transform: rotate(0deg);
}

#cardFront,
#cardBack {
  backface-visibility: hidden;
  width: 100%;
  height: 100%;
  position: absolute;
}

#cardFront {
  z-index: 100;
}

#cardBack {
  z-index: 101;
  transform: rotateY(180deg);
  background: grey;
  overflow: hidden;
  border-radius: 5px;

  .scrollContainer {
    height: 100%;
  }
}
</style>
