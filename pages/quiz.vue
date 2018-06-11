<template>
  <section class="question-wrapper">
    Current Question: {{ currentQuestion }}
    Current Answer: {{ currentAnswer }}
    <div
      v-for="(question, questionIndex) in questions"
      v-if="currentQuestion === questionIndex + 1"
      :key="questionIndex"
      class="question-step">
      <p class="question-label">{{question.label}}</p>
      <label
        v-for="(answer, answerIndex) in question.answers"
        :key="answerIndex"
        :for="setAnswerId(questionIndex, answerIndex)">
        <input
          type="radio"
          v-model="currentAnswer"
          :value="answerIndex + 1"
          :name="'answer-' + (questionIndex + 1)"
          :id="setAnswerId(questionIndex, answerIndex)">
        {{answer.label}}
      </label>
    </div>
    <button @click="validateQuestion">Valider</button>
  </section>
</template>

<script>

export default {
  data() {
    return {
      currentQuestion: 1,
      currentAnswer: null,
      questions: [
        {
          label: 'Question 1 ?',
          answers: [
            { label: 'Réponse 1', correct: false },
            { label: 'Réponse 2', correct: true },
            { label: 'Réponse 3', correct: false },
            { label: 'Réponse 4', correct: false }
          ]
        },
        {
          label: 'Question 2 ?',
          answers: [
            { label: 'Réponse 1', correct: false },
            { label: 'Réponse 2', correct: true },
            { label: 'Réponse 3', correct: false },
            { label: 'Réponse 4', correct: false }
          ]
        }
      ],
      user: {
        answers: [],
        score: 0
      }
    }
  },
  methods: {
    setAnswerId: function(questionIndex, answerIndex) {
      return `q${questionIndex}-a${++answerIndex}`;
    },
    validateQuestion: function() {
      this.setUserAnswer();
      this.currentAnswer = null;

      if (this.currentQuestion === this.questions.length) {
        console.log('end quiz')
      } else {
        this.currentQuestion ++;
      }
    },
    setUserAnswer: function() {
      let isCorrect = this.questions[this.currentQuestion - 1].answers[this.currentAnswer - 1].correct;
      if (isCorrect) this.user.score ++;

      let questionResult = {
        answer: this.currentAnswer,
        correct: isCorrect
      }
      this.user.answers.push(questionResult);
    }
  }
}
</script>

<style>
.question-wrapper {
  text-align: center;
}
.question-label {
  margin-bottom: 30px;
}
label {
  display: block;
}
</style>
