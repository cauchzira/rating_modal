<template>
  <div class="">
    <v-btn elevation="2" @click="openModal">Оставить отзыв</v-btn>
    <FeedbackModal />
  </div>
</template>

<script>
import FeedbackModal from "@/components/Modals/FeedbackModal.vue";

export default {
  name: "HomeView",
  components: {
    FeedbackModal,
  },
  methods: {
    openModal() {
      this.$root.$emit("openModal");
    },
  },
};
</script>
