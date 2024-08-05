<template>
  <div>
    <RateBox v-model="rating" :editable="true" />
    <p>{{ rating }}</p>
  </div>
</template>

<script>
import RateBox from "./components/RateBox.vue";

export default {
  components: {
    RateBox,
  },
  data() {
    return {
      rating: 0,
    };
  },
};
</script>
