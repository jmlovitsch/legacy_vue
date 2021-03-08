<template>
  <div id="app">
    <b-container>
      <b-row>
        <b-col sm="6" offset="3">
          <b-alert
            class="alert-over"
            v-model="showDismissibleAlert"
            dismissible
          >
            `We are sorry but there are no results that fit those parameters.
            Please change parameters and resubmit. We apologize for the
            inconvience.`
          </b-alert>

          <Header :numCorrect="numCorrect" :numTotal="numTotal" />
          <ChooseLength
            v-if="!quizLength"
            :fetchRequest="fetchRequest"
            :categories="categories"
          />
          <QuestionBox
            v-else-if="questions.length && !quizEndedVar"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
            :isItLast="isItLast"
          />
          <Finale
            v-else-if="quizEndedVar"
            :numCorrect="numCorrect"
            :numTotal="numTotal"
            :restartGame="restartGame"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";
import Finale from "@/components/Finale.vue";
import ChooseLength from "@/components/ChooseLength.vue";
// import NoResults from "@/components/NoResults.vue"
export default {
  name: "app",
  components: {
    Header,
    QuestionBox,
    Finale,
    ChooseLength,
    // NoResults,
  },
  data() {
    return {
      quizLength: 0,
      quizCategory: "",
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0,
      quizEndedVar: false,
      numberOfPlayers: 1,
      categories: [],
      showDismissibleAlert: false,

      players: [
        {
          name: "" || "Player 1",
          score: 0,
        },
        {
          name: "" || "Player 2",
          score: 0,
        },
        {
          name: "" || "Player 3",
          score: 0,
        },
        {
          name: "" || "Player 4",
          score: 0,
        },
      ],
    };
  },
  methods: {
    next(event) {
      console.log(event.target);
      if (event.target.innerText === "Finish") {
        this.quizEndedVar = true;
      }
      this.index++;
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++;
      }
      this.numTotal++;
    },
    isItLast() {
      if (this.questions.length - 1 === this.index) {
        return true;
      }
      false;
    },
    fetchRequest({ number, category, type, difficulty }) {
      this.quizLength = number;
      console.log(
        `https://opentdb.com/api.php?amount=${number}` +
          (category ? `&category=${category}` : "") +
          (difficulty ? `&difficulty=${difficulty}` : "") +
          (type ? `&type=${type}` : "")
      );
      fetch(
        `https://opentdb.com/api.php?amount=${number}` +
          (category ? `&category=${category}` : "") +
          (difficulty ? `&difficulty=${difficulty}` : "") +
          (type ? `&type=${type}` : ""),
        {
          method: "get",
        }
      )
        .then((response) => {
          return response.json();
        })
        .then((jsonData) => {
          if (jsonData.response_code === 1) {
            this.showDismissibleAlert = true;
            //    alert("We are sorry but there are no results that fit those parameters. \n Please change parameters and resubmit. \n We apologize for the inconvience.")
            return this.restartGame();
          }
          console.log(jsonData);
          this.questions = jsonData.results;
        });
    },
    restartGame() {
      (this.quizLength = 0),
        (this.questions = []),
        (this.index = 0),
        (this.numCorrect = 0),
        (this.numTotal = 0),
        (this.quizEndedVar = false);
    },
  },

  mounted: function () {
    fetch("https://opentdb.com/api_category.php", {
      method: "get",
    })
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        jsonData.trivia_categories.map((category) => {
          this.categories.push(category);
        });
      });
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.alert-over {
  font-weight: 600;
  background-color: indianred;
  top: 50%;
  position: absolute;
  justify-content: center;
  align-items: center;
  z-index: 11111;
}
</style>
