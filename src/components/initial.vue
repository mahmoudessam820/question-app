<template>
  <div>
    <h1>Ask a question</h1>

    <div class="mb-3">
      <input
        name="question"
        type="text"
        class="form-control"
        id="question"
        v-model="question"
      />

      <button
        v-if="question"
        class="btn animate__animated animate__fadeIn"
        @click="handleNext"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Initial",
  data() {
    return {
      question: "",
    };
  },
  methods: {
    handleNext(e) {
      e.preventDefault();
      if (this.question.length <= 5) {
        this.$emit("handleError", {
          type: "error",
          message: "Your question is too short",
        });
      } else if (this.question.length >= 50) {
        this.$emit("handleError", {
          type: "error",
          message: "Your question is too long",
        });
      } else {
        /*
          - emit() will make an event that will change whatever the page and go to the next page.
          - It is that we want to change right here.
          - $emit('goto', 1)
          -- [goto] == this is the name of my reference.
          -- [ 1 ]  == We want to go to we need to go to where we useing position from the parent.
          -- [ 1 ]  == One is going to let the parents know the app go to a place and that place is going to the number one.
        */
        this.$emit("goto", 1);
        this.$emit("question", this.question);
      }
    },
  },
};
</script>

<style>
</style>