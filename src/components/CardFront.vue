<template>
  <div class="card" :style="{ 'background-color': backgroundColor }" @click="$emit('cardClicked')">
    <span>{{ colorName }}</span>
  </div>
</template>

<script>
export default {
  name: "CardFront",
  props: ["backgroundColor", "colorName"]
};
</script>

<style lang="scss">
.card {
  padding-top: 100px;
  padding-bottom: 100px;
  cursor: pointer;

  span {
    color: #fff;
    font-weight: bold;
    font-size: 18px;
  }
}
</style>
