<template>
  <div class="container">
    <transition name="fade" appear mode="out-in">
      <!-- 
        listening to this event for something called [@goto, @question, ..] it coming from the Children.
      -->
      <component
        :is="screens[position]"
        :question="question"
        :result="result"
        @goto="handleGoTo"
        @question="handleQuestion"
        @showResult="showResult"
        @startOver="startOver"
        @handleError="handleError"
      />
    </transition>
  </div>
</template>

<script>
import appInital from "./components/initial.vue";
import appConfirm from "./components/confirm.vue";
import appResults from "./components/results.vue";

export default {
  name: "App",
  components: {
    appInital,
    appConfirm,
    appResults,
  },
  data() {
    return {
      list: [
        "Yes",
        "No",
        "Not sure..try again",
        "Ask a friend",
        "Call the police",
      ],
      screens: ["appInital", "appConfirm", "appResults"],
      position: 0,
      question: "",
      result: "",
    };
  },
  methods: {
    handleError(values) {
      this.$toast.show(values.message, {
        type: values.type,
        position: 'top',
        duration: 2000,
        pauseOnHover: false,
      });
    },
    handleGoTo(position) {
      this.position = position;
    },
    handleQuestion(question) {
      this.question = question;
    },
    getRandomValue() {
      return this.list[Math.floor(Math.random() * this.list.length)];
    },
    generateResult() {
      let rand = this.getRandomValue();

      if (this.result !== "") {
        while (rand === this.result) {
          rand = this.getRandomValue();
        }
      }

      this.result = rand;
    },
    showResult() {
      this.generateResult();
    },
    startOver() {
      this.position = 0;
      this.question = "";
      this.result = "";
    },
  },
};
</script>

<style>
@import "./assets/style.css";

.fade-enter-from {
  opacity: 0;
}

.fade-enter-active {
  transition: 0.5s;
}

.fade-enter-active {
  opacity: 1;
}

.fade-leave-from {
  opacity: 1;
}

.fade-leave-active {
  transition: 0.5s;
}

.fade-leave-active {
  opacity: 0;
}
</style>