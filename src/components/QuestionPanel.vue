<script>
export default {
  name: "QuestionPanel",
  data() {
    return {
      picked: null,
    };
  },
  props: {
    question: {
      type: Object,
      default: null,
    },
    isSubmitted: {
      type: Boolean,
      default: false,
    },
    isSubmissionAttempted: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    isCorrect() {
      return (
        this.picked &&
        this.isSubmitted &&
        this.question.correctAnswerId === this.picked
      );
    },
    isWrong() {
      return (
        this.picked &&
        this.isSubmitted &&
        this.question.correctAnswerId !== this.picked
      );
    },
    feedbackColor() {
      if (this.isSubmissionAttempted && this.picked === null) {
        return "yellow";
      } else if (this.isSubmitted) {
        return this.isCorrect ? "#07A006" : "red";
      } else {
        return null;
      }
    },
  },
};
</script>

<template>
  <div
    v-if="question"
    class="panel-wrapper"
    :style="
      isSubmissionAttempted || isSubmitted
        ? `border: 2px solid ${feedbackColor} ;`
        : ''
    "
  >
    <div>
      {{ `${question.id}. ${question.text}` }}
    </div>
    <div class="answers-wrapper">
      <div
        v-for="answer of question.answers"
        class="a-row-wrapper"
        :key="answer.id"
        :class="{
          'correct-ans': isWrong && question.correctAnswerId === answer.id,
        }"
      >
        <div class="rad-btn">
          <input
            type="radio"
            :id="question.id + answer.id"
            :value="answer.id"
            v-model="picked"
            @click="
              $emit('update-answers', {
                questionId: question.id,
                answerId: answer.id,
              })
            "
          />
        </div>
        <label class="answer-text" :for="question.id + answer.id">{{
          answer.text
        }}</label>
      </div>
    </div>
    <div
      class="feedback-msg"
      :style="`color:${feedbackColor}`"
      v-if="isSubmitted"
    >
      {{ isCorrect ? "Correct" : "Wrong" }}
    </div>
  </div>
</template>

<style scoped>
.panel-wrapper {
  margin: 10px 0 10px 0;
  background-color: white;
  border-radius: 10px;
  padding: 20px;
  box-shadow: rgba(0, 0, 0, 0.12) 0px 1px 3px, rgba(0, 0, 0, 0.24) 0px 1px 2px;
}

.a-row-wrapper {
  padding: 7px 0 5px 10px;
  max-width: 75%;
  display: flex;
}
.answers-wrapper {
  padding-left: 15px;
}
.answer-text {
  padding-left: 10px;
}
.feedback-msg {
  position: absolute;
  bottom: 10px;
  right: 10px;
}
.correct-ans {
  background-color: #07a006;
  border-radius: 10px;
  color: white;
  border: 2px solid darkgreen;
}
.rad-btn {
  align-self: flex-end;
}
@media (max-width: 1180px) {
  .answers-wrapper {
    padding-left: 0;
    margin-left: -10px;
  }
}
</style>
