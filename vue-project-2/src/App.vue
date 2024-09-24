

<template>
  <header>
    <h2>Flash Word</h2>

    <p v-if="completed" id="completed">good work, you completed all words!!</p>
    <P v-else-if="correctCount != null" id="correctCount"
      >you have answered {{ correctCount }} out of {{ WordCount }}</P
    >
    <P v-if="incorrectCount != null" id="correctCount"
      >you have answered wrong{{ incorrectCount }} out of {{ WordCount }}</P
    >
    <div id="cards">
      <WordCard
        v-for="word in shuffledWords"
        v-bind:word="word"
        v-on:incrementCorrectCount="incrementCorrectCount"
      ></WordCard>
    </div>

    <button @click="filterByDifficulty('easy')">Easy</button>
    <button @click="filterByDifficulty('medium')">Medium</button>
    <button @click="filterByDifficulty('hard')">Hard</button>

    <div>
      <button @click="resetBtn()">reset all</button>
      <button @click="resetWrongAnswer()">reset wrong answer</button>
    </div>
  </header>

  <!-- <RouterView /> -->
</template>

<script >
import { RouterLink, RouterView } from "vue-router";
import WordCard from "./components/WordCard.vue";

export default {
  components: { WordCard },
  data() {
    return {
      EnglishWords: [
        {
          word_A: "hola",
          word_B: "hello",
          Hint: "whats`s app",
          Answer: "",
          correct: false,
        },
        {
          word_A: "uno",
          word_B: "one",
          Hint: "number",
          Answer: "",
          correct: false,
        },
      ],
      correctCount: null,
      incorrectCount: null,
      completed: false,

      filteredWords: [],
      shuffledWords: null,
    };
  },

  mounted() {},

  computed: {
    ShuffledWords() {
      if (!this.shuffledWords) {
        this.shuffledWords = this.EnglishWords.sort(() => Math.random() - 0.5);
      }
      return this.shuffledWords;
    },

    // sorting(){
    //     this.ShuffledWords=arr.sort((a,b) => b.length - a.length);
    // },
    WordCount() {
      return this.EnglishWords.length;
    },
  },
  // this methods watching correctCount in method checkanswer order to if it equal versus the WordCount say alert congrats
  watch: {
    // correctCount() {
    //     if (this.correctCount == this.WordCount) {
    //         alert('congrats');
    //     }
    // },

    // Watcher for correctCount.
    // If correctCount equals WordCount, sets completed flag to true.
    correctCount() {
      this.completed = this.correctCount == this.WordCount;
    },

    /**
     * Automatically resets the game if the user makes two incorrect
     * answers in a row. This is a safety feature to prevent the user from
     * getting stuck in a loop of incorrect answers.
     */
    incorrectCount() {
      if (this.incorrectCount >= 10 || this.index) {
        this.resetBtn();
        alert("please be more careful and strive to answer correctly");
      }
      this.EnglishWords.forEach((word) => {
        if (this.incorrectCount >= 2) {
          word.Answer = "";
        }
      });
    },
  },

  methods: {
    /**
     * Checks if the user's answer matches the correct answer.
     * If correct, increments the correctCount and marks the word as correct.
     * If incorrect, marks the word as incorrect.
     * @param {Object} word - The object containing the word's data.
     */

    resetBtn() {
      this.ShuffledWords.forEach((word) => {
        word.Answer = "";
        word.correct = false;
        word.incorrect = false;
        this.completed = false;
        this.correctCount = null;
        this.incorrectCount = null;
      });
    },

    resetWrongAnswer() {
      this.ShuffledWords.forEach((word) => {
        if (word.incorrect) {
          word.Answer = "";
          word.incorrect = false;
          this.incorrectCount = null;
        }
      });
    },

    filterByDifficulty(level) {
      if (level === "easy") {
        return this.EnglishWords.filter((word) => word.word_A.length <= 3);
      } else if (level === "medium") {
        return this.EnglishWords.filter(
          (word) => word.word_A.length > 3 && word.word_A.length <= 6
        );
      } else if (level === "hard") {
        return this.EnglishWords.filter((word) => word.word_A.length > 6);
      }
    },
  },
};
</script>

<style>
header {
  line-height: 1.5;
  max-height: 100vh;
}

[v-cloak] {
  display: none;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin-top: 60px;
}

#cards {
  justify-content: center;
  display: grid;
  grid-template-columns: 350px 350px 350px;
  grid-gap: 30px;
}

button {
  margin: 15px;
  margin-top: 3em;
  padding: 10px;
  color: blue;
  background-color: #e8f0ff;
  border: 0;
  font-size: 20px;
  border-radius: 5px;
}

#correctCount {
  font-size: 20px;
  margin: 10px;
  font-weight: bold;
  padding: 10px;
}

#completed {
  font-size: 20px;
  font-weight: bold;
  color: #0f5132;
  padding: 10px;
  margin: 10px;
}
</style>
