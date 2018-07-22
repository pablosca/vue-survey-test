<template>
  <div id="app">
    <Header />
    <keep-alive>
      <component :is="activeScreen" v-bind="activeData" v-if="!loading && !hasErrors"></component>
    </keep-alive>
    <div v-if="hasErrors" class="error">Failed to connect to the server.</div>
  </div>
</template>

<script>
  import axios from 'axios';
  import Header from '@/components/Header';
  import Welcome from '@/views/Welcome';
  import Questionary from '@/views/Questionary';
  import Report from '@/views/Report';

  export default {
    components: { Header, Welcome, Questionary, Report },

    data () {
      return {
        activeScreen: 'Welcome',
        candidate: null,
        questions: null,
        hasErrors: false,
        loading: true,
        answered: []
      };
    },

    computed: {
      activeData () {
        switch (this.activeScreen) {
          case 'Welcome': return { candidate: this.candidate };
          break;
          case 'Questionary': return { questions: this.questions, answered: this.answered };
          break;
          case 'Report': return { questions: this.questions, answered: this.answered, candidate: this.candidate };
          break;
        }
      }
    },

    watch: {
      answered (newValue) {
        if (newValue.length === this.questions.length) {
          this.activeScreen = 'Report';
        }
      }
    },

    created () {
      axios.get('/api-data.json')
        .then(response => {
          this.candidate = response.data.candidate;
          this.questions = response.data.questions;
          this.loading = false;
        })
        .catch(error => {
          this.hasErrors = true;
        });
    },
    mounted () {
      this.$bus.$on('screen:change', e => {
        this.activeScreen = e.screen;
      });

      this.$bus.$on('question:answered', newAnswer => {
        const answerIndex = this.answered.findIndex(item => item.questionId === newAnswer.questionId);

        // check if quesiton is already answered
        if (answerIndex === -1) {
          this.answered.push(newAnswer);
        } else {
          // replace it if its already there
          this.$set(this.answered, answerIndex, newAnswer);
        }
      });
    }
  }
</script>

<style>
  @import './assets/styles/style.css';
</style>
