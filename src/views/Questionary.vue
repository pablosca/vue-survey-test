<template>
  <div class="questionary">
    <div class="questions">
      <div
        v-for="(question, index) in questions"
        :key="question.question_id"
        class="question"
        v-if="index === currentQuestionIndex"
      >
      
        <progress-status :current="index" :length="questions.length" />

        <question
          :question="question"
          :answer="answered[index]"
          @answered="onQuestionAnswered"
        />
        <div class="footer contain wider" v-show="currentQuestionIndex">
            <a @click="goToPreviousQuestion" tabindex="0" class="prevbtn button secondary">Previous</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Question from '@/components/Question';
import ProgressStatus from '@/components/ProgressStatus';

export default {
  components: { Question, ProgressStatus },
  props: ['questions', 'answered'],

  data () {
    return {
      group_id: null,
      candidate_id: null,
      secret: null,
      test_id: null,
      currentQuestionIndex: 0,
      candidate: {}
    };
  },

  methods: {
    onQuestionAnswered (answer) {
      this.$bus.$emit('question:answered', answer);
      this.currentQuestionIndex++;
    },
    goToPreviousQuestion () {
      this.currentQuestionIndex--;
    }
  }
}
</script>
