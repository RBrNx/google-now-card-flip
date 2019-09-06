<template>
  <div class="home">
    <h1>Google Now Card Flip</h1>
    <div class="cardsContainer">
      <card-front
        v-for="card in cards"
        :key="card.id"
        :id="card.id"
        :backgroundColor="card.backgroundColor"
        :colorName="card.colorName"
        @cardClicked="turnCard(card.id)"
      ></card-front>
      <card-flip v-if="chosenCard" :cardId="chosenCard.id" @cardClosed="unmountCard">
        <template v-slot:cardFront>
          <card-front
            :backgroundColor="chosenCard.backgroundColor"
            :colorName="chosenCard.colorName"
          ></card-front>
        </template>
        <template v-slot:cardBack>
          <div class="cardBack">
            <span>{{ chosenCard.description }}</span>
          </div>
        </template>
      </card-flip>
    </div>
  </div>
</template>

<script>
import CardFront from "../components/CardFront";
import CardFlip from "../components/CardFlip";

export default {
  name: "home",
  components: {
    CardFront,
    CardFlip
  },
  methods: {
    turnCard(id) {
      const card = this.cards.find(p => p.id === id);
      this.chosenCard = card;
    },
    unmountCard() {
      this.chosenCard = null;
    }
  },
  data() {
    return {
      cards: [
        {
          id: "Card-1",
          backgroundColor: "#E01A4F",
          colorName: "Spanish Crimson",
          description:
            "In a RGB color space, hex #e01a4f is composed of 87.8% red, 10.2% green and 31% blue. Whereas in a CMYK color space, it is composed of 0% cyan, 88.4% magenta, 64.7% yellow and 12.2% black. It has a hue angle of 343.9 degrees, a saturation of 79.2% and a lightness of 49%. #e01a4f color hex could be obtained by blending #ff349e with #c10000. Closest websafe color is: #cc3366."
        },
        {
          id: "Card-2",
          backgroundColor: "#F15946",
          colorName: "Orange Soda",
          description:
            "In a RGB color space, hex #f15946 is composed of 94.5% red, 34.9% green and 27.5% blue. Whereas in a CMYK color space, it is composed of 0% cyan, 63.1% magenta, 71% yellow and 5.5% black. It has a hue angle of 6.7 degrees, a saturation of 85.9% and a lightness of 61%. #f15946 color hex could be obtained by blending #ffb28c with #e30000. Closest websafe color is: #ff6633."
        },
        {
          id: "Card-3",
          backgroundColor: "#F9C22E",
          colorName: "Saffron",
          description:
            "In a RGB color space, hex #f9c22e is composed of 97.6% red, 76.1% green and 18% blue. Whereas in a CMYK color space, it is composed of 0% cyan, 22.1% magenta, 81.5% yellow and 2.4% black. It has a hue angle of 43.7 degrees, a saturation of 94.4% and a lightness of 57.8%. #f9c22e color hex could be obtained by blending #ffff5c with #f38500. Closest websafe color is: #ffcc33."
        }
      ],
      chosenCard: null
    };
  }
};
</script>

<style lang="scss">
@import "../global.scss";

.cardsContainer {
  padding: 0 25px;
  position: relative;
  display: grid;
  grid-template-columns: repeat(1, 1fr);
  grid-gap: 30px;

  @include tablet {
    padding: 0 45px;
    grid-template-columns: repeat(2, 2fr);
  }

  @include laptop {
    padding: 0 100px;
    grid-template-columns: repeat(3, 2fr);
  }

  @include desktop {
    padding: 0 175px;
    grid-template-columns: repeat(3, 2fr);
  }

  .cardBack {
    background-color: gray;
    color: #fff;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 25px;
  }
}
</style>
