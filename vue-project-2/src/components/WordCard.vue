




<template>
  <div id="cards">
    <div
      class="card"

      
      
      v-bind:class="{ correct: word.correct, incorrect: word.incorrect }">
      
      <p class="word">{{ word.word_A }}</p>
      
      <input
      type="text"
      v-if="!word.correct"
      v-model="word.Answer"
      @keyup.enter="CheckAnswer()"
      />
      
      <p v-else class="correct.word_B">{{ word.word_B }}</p>
      <p v-if="word.incorrect == incorrectCount >= 2">{{ word.Hint }}</p>
    </div>
  </div>
</template>


<!-- v-for="word in ShuffledWords" -->



<script>
export default {
  name: "WordCard",
  props: ["word", "x", "y"],
  data() {
    return {};
  },
  mounted() {},
  computed: {},
  watch: {},
  methods: {
    CheckAnswer() {
      this.word.correct = this.word.word_B == word.Answer;
      this.word.incorrect = this.word.word_B !== word.Answer;
    },
    
    incrementCorrectCount() {
      if (word.correct) {
        this.correctCount++;
      } else {
        this.incorrectCount++;
      }
    },
  },
};
</script>





<style scoped>
.card {
  background-color: #E8F0FF;
  border-radius: 5px;
  padding: 10px 0;
  font-size: 25px;
}

input[type=text] {
  border: 0;
  font-size: 25px;
  border-radius: 5px;
  margin-top: 5px;
  text-align: center;
  padding: 5px;
}

.word {
  font-weight: bold;
  padding: 0;
  margin: 0;
}

.correctAnswer {
  padding: 0;
  margin: 0;
}
.correctanswer {
  padding: 0;
  margin: 0;
}

.correct {
  color: #0f5132;
  background-color: #d1e7dd;
}

.incorrect {
  color: #db4343;
  background-color: #E8F0FF;
}
</style>