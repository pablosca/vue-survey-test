<template>
    <div>
        <h1 class="title contain wider multirow">{{ question.value }}</h1>
        <div class="answers_values contain wider">
            <div
                class="answer"
                :class="{
                    'loading': isLoading && clickedAnswer === answer.id,
                    'active': activeAnswerId === answer.id
                }"
                tabindex="0"
                v-for="answer in answers"
                :key="answer.id"
                @click="answerQuestion(answer.id)"
                @keyup.enter="answerQuestion(answer.id)"
            >
                <span class="value">{{ answer.text }}</span>
                <span class="icon">Submitting...</span>
            </div>
        </div>
    </div>
</template>

<script>
import answers from '@/answers';

export default {
    props: ['question', 'answer'],
    data () {
        return {
            answers,
            clickedAnswerId: null,
            isLoading: false
        };
    },
    computed: {
        activeAnswerId () {
            return this.answer ? this.answer.answerId : null;
        }
    },
    methods: {
        answerQuestion (answerId){
            const newAnswer = {
                answerId,
                questionId: this.question.question_id
            };
            if (this.loading) {
                return false;
            }
            
            this.clickedAnswer = answerId;
            this.isLoading = true;

            setTimeout(() => {
                this.$emit('answered', newAnswer);
                this.isLoading = false;
                this.clickedAnswer = null;
            }, 800)
        }
    }
}
</script>

