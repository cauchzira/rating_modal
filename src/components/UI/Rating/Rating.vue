<template>
  <div class="star-rating" @mouseleave="leaveHandler">
    <span
      v-for="index in starLimit"
      :key="index"
      class="star star--outlined"
      @click.stop="rate(index)"
      @mousemove="hoverHandler(index)"
    ></span>
    <div
      class="star-rating__colored"
      :style="`width: ${
        ratingHoveredWidth !== '0%' ? ratingHoveredWidth : ratingWidth
      }`"
      @mouseleave="leaveHandler"
    >
      <span
        v-for="index in starLimit"
        :key="index"
        class="star star--colored"
        @click.stop="rate(index)"
        @mousemove="hoverHandler(index)"
      ></span>
    </div>
  </div>
</template>

<script>
export default {
  name: "RatingStar",
  props: {
    rating: {
      type: Number,
      default: 3,
    },
    starLimit: {
      type: Number,
      default: 5,
    },
    fullIcon: {
      type: String,
    },
    emptyIcon: {
      type: String,
    },
  },
  data() {
    return {
      stars: this.rating,
      hovered: 0,
    };
  },
  methods: {
    rate(star) {
      if (typeof star === "number" && star <= this.starLimit && star >= 0) {
        this.stars = star;
        this.$root.$emit("updateStars", this.stars);
      }
    },
    hoverHandler(star) {
      if (typeof star === "number" && star <= this.starLimit && star >= 0) {
        this.hovered = star;
      }
    },
    leaveHandler() {
      this.hovered = 0;
    },
  },
  computed: {
    ratingWidth() {
      return (this.stars / this.starLimit) * 100 + "%";
    },
    ratingHoveredWidth() {
      return (this.hovered / this.starLimit) * 100 + "%";
    },
  },
  mounted() {
    this.$root.$emit("updateStars", this.stars);
  },
};
</script>

<style lang="scss" scoped>
@import "./Rating.scss";
</style>
