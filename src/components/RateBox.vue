<template>
  <div class="rate-box" @mouseleave="resetHover">
    <div
      v-for="star in stars"
      :key="star"
      class="star"
      :class="{ filled: star <= (hoverRating || rating) }"
      @mouseover="setHover(star)"
      @click="handleClick(star)"
    >
      ★
    </div>
  </div>
</template>

<script>
import { defineComponent, ref, watch, computed } from "vue";

// Composition Api Used
export default defineComponent({
  name: "RateBox",
  props: {
    modelValue: {
      type: Number,
      default: 0,
    },
    editable: {
      type: Boolean,
      default: false,
    },
  },
  setup(props, { emit }) {
    const rating = ref(props.modelValue);
    const hoverRating = ref(0);
    const stars = [1, 2, 3, 4, 5];

    // Watch the changes
    watch(
      () => props.modelValue,
      (newValue) => {
        rating.value = newValue;
      }
    );

    // Update filled stars
    const filledStars = computed(() => hoverRating.value || rating.value);

    const setHover = (star) => {
      if (props.editable) {
        hoverRating.value = star;
      }
    };

    const resetHover = () => {
      if (props.editable) {
        hoverRating.value = 0;
      }
    };

    // Click star and see the value
    const handleClick = (star) => {
      const newRating = rating.value === star ? 0 : star;
      rating.value = newRating;
      emit("update:modelValue", newRating);
    };

    return {
      stars,
      rating,
      hoverRating,
      filledStars,
      setHover,
      resetHover,
      handleClick,
    };
  },
});
</script>

<style scoped>
.rate-box {
  display: flex;
}

.star {
  font-size: 24px;
  cursor: pointer;
  transition: color 0.2s ease;
}

.star.filled {
  color: gold;
}
</style>
