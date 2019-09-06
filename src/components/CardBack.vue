<template>
  <div class="cardBack">
    <span>{{ description }}</span>
  </div>
</template>

<script>
export default {
  name: "CardBack",
  props: ["description"]
};
</script>

<style lang="scss">
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
</style>
