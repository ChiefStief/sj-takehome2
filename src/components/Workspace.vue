<script>
import QuestionPanel from "./QuestionPanel.vue";

export default {
  name: "WorkSpace",
  components: { QuestionPanel },
  props: {
    questions: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      answers: {},
      isSubmissionAttempted: false,
      isSubmitted: false,
    };
  },
  computed: {
    areUnanswered() {
      return Object.entries(this.answers).length < this.questions.length;
    },
    numRight() {
      return this.questions.filter((question) => {
        return question.correctAnswerId === this.answers[question.id];
      }).length;
    },
    percentRight() {
      return Math.round((this.numRight * 100) / this.questions.length);
    },
  },
  methods: {
    updateAnswers({ questionId, answerId }) {
      this.isSubmissionAttempted = false;
      this.isSubmitted = false;
      this.answers[questionId] = answerId;
    },
    onSubmit() {
      if (this.areUnanswered) {
        this.isSubmissionAttempted = true;
      } else {
        this.isSubmitted = true;
      }
    },
  },
};
</script>

<template>
  <div class="workspace-wrapper">
    <div class="quiz-header">Quiz 1 - HTML / CSS / JS Practice</div>
    <div v-if="isSubmitted" class="results-wrapper">
      <div class="results-title">Results</div>
      <div class="results-text">
        You got {{ numRight }}/{{ questions.length }} questions correct
      </div>
      <div class="results-percent">{{ percentRight }}%</div>
    </div>
    <QuestionPanel
      v-for="question of questions"
      :key="question.id"
      :is-submitted="isSubmitted"
      :is-submission-attempted="isSubmissionAttempted"
      :question="question"
      :answer-given="answers[question.id] || null"
      @update-answers="updateAnswers"
    />
    <div class="submission-blk">
      <button type="button" class="submit-btn" @click="onSubmit">Submit</button>
      <div v-if="isSubmissionAttempted" class="unanswered-block">
        Answer all questions before submitting. Unanswered questions are
        displayed in yellow.
      </div>
    </div>
  </div>
</template>

<style scoped>
.workspace-wrapper {
  padding: 40px 10px 30px 10px;
  max-width: 75%;
  margin: auto;
}
.submit-btn {
  background-color: green;
  border-radius: 4px;
  color: white;
  padding: 10px;
  border: 2px solid black;
}
.submission-blk {
  text-align: center;
}
.unanswered-block {
  text-align: center;
  color: red;
  padding-top: 10px;
}
.results-title {
  text-decoration: underline;
  padding-bottom: 10px;
}

.results-percent {
  color: red;
  font-weight: bolder;
}

.results-wrapper {
  text-align: center;
}

.quiz-header {
  font-size: 2rem;
}
@media (max-width: 1180px) {
  .workspace-wrapper {
    max-width: 90%;
    margin-left: 0;
    padding-left: 20px;
  }
  .quiz-header {
    padding-left: 30px;
    font-size: 1.5rem;
  }
}

@media (max-width: 767px) {
  .workspace-wrapper {
    max-width: 100%;
  }
  .quiz-header {
    padding-left: 0;
  }
}
</style>
