<template>
  
  <div>
    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount"/>
    <template v-if="this.question">
      <h1 v-html="this.question">   
      </h1>
      <template v-for="(option,index) in this.options" :key="index">
      <input type="radio" name="options" :value="option" v-model="this.chosen_option" :disabled="this.answerSubmitted">
      <label v-html="option"></label> <br>
    </template>
    <button class="send" type="button" @click="check_answer()" v-if="!this.answerSubmitted">Send</button>  
    </template>

    <section class="result" v-if="this.answerSubmitted">
      <h4 v-if="this.chosen_option==this.correctAnswers">&#9989; You have picked the correct answer!</h4>
      <h4 v-else>&#10060; You have picked the wrong answer! The correct answer is "{{ this.correctAnswers }}"</h4>
      <button class="send" type="button" @click="getNewQuestion()">Next Question</button>
    </section>
  </div>
  
  
</template>

<script>
import ScoreBoard from './components/ScoreBoard.vue';
export default {
  name: 'App',
  components:{
    ScoreBoard
  },
  data(){
    return{
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswers: undefined,
      chosen_option: undefined,
      answerSubmitted: false,
      winCount: 0,
      loseCount: 0
    }
  },
  methods:{
    check_answer(){
      if(!this.chosen_option){
        alert('choose an option');
      }
      else{
        this.answerSubmitted=true;
        if(this.chosen_option==this.correctAnswers){
          this.winCount++;
        }
        else{
          this.loseCount++;
        }
      }
    },
    getNewQuestion(){
      this.answerSubmitted=false;
      this.question=undefined;
      this.chosen_option=undefined;

      this.axios
      .get('https://opentdb.com/api.php?amount=1&category=9')
      .then((response) => {
        this.question=response.data.results[0].question;
        this.incorrectAnswers=response.data.results[0].incorrect_answers;
        this.correctAnswers=response.data.results[0].correct_answer;
    });
  }
},
  computed:{
    options(){
      var options=[...this.incorrectAnswers];
      options.splice(Math.round(Math.random() * options.length),0,this.correctAnswers);
      return options;
    }
  },
  created(){
    this.axios
    .get('https://opentdb.com/api.php?amount=10&category=20')
    .then((response) => {
        this.question=response.data.results[0].question;
        this.incorrectAnswers=response.data.results[0].incorrect_answers;
        this.correctAnswers=response.data.results[0].correct_answer;
        
      })
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type=radio]{
    margin: 12px 4px;
  }

  button.send{
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
