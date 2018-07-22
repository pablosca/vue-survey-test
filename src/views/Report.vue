<template>
    <div class="report">
        <div class="greeting">
        <div class="contain">
            <h1 class="hey">Very Simple Report</h1>
            <div class="report-container">
                <p><strong>{{ candidate.name }}</strong>'s report:</p>
                <div>
                    You answered a total of <strong>{{ questions.length }} questions</strong>,
                    find below your very simple report.
                </div>
                <br />
                <h1 class="title">How many times for each answer:</h1>
                <PieChart :data="chartData" />

                <div class="report-items">
                    <question
                        v-for="(question, index) in questions"
                        :question="question"
                        :answer="answered[index]"
                        :key="index"
                    />
                </div>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
import possibleAnswers from '@/answers';

import PieChart from '@/components/PieChart';
import Question from '@/components/Question';

const colors = [
    'rgb(255, 159, 64)',
    'rgb(255, 205, 86)',
    'rgb(0, 163, 51)',
    'rgb(54, 162, 235)',
    'rgb(153, 102, 255)',
    'rgb(182, 54, 197)'
];

export default {
    props: [ 'candidate', 'questions', 'answered' ],
    components: { PieChart, Question },

    computed: {
        chartData () {
            return {
                datasets: [{
                    data: this.answersCount,
                    backgroundColor: colors
                }],

                labels: this.answersLabels
            }
        },

        // return all the answers in an object of {answerId: answerCount, ...}
        answersObject () {
            var data = {};

            for(let i = 0; i < this.answered.length; i++) {
                let item = this.answered[i];

                if (data[item.answerId]) {
                    data[item.answerId]++;
                } else {
                    data[item.answerId] = 1;
                }
            }
            return data;

        },
        
        // For the chart: get only the count for each answers
        answersCount () {
            return Object.values(this.answersObject);
        },

        // For the chart: get the labels (answer text)
        answersLabels () {
            let answersIds = Object.keys(this.answersObject);

            return answersIds.map(id => {
                return possibleAnswers.find(answer => answer.id === parseInt(id)).text;
            })
        }
    }
}
</script>


<style>
.hey {
    text-align: center;
}
.report-container {
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 6px;
    margin: 0 auto;
    max-width: 750px;
    padding: 30px;
}
.report .report-items {
    margin-top: 4rem;
}
.report .report-items .contain {
    padding: 0;
}
.report .report-items .title {
    font-size: 22px;
    margin-bottom: 10px;
    min-height: 0;
    padding: 0;
}
.report-items .answers_values {
    margin-bottom: 4em;
}
.report .report-items .answer {
    border-color: #ccc;
    color: #ccc;
}
</style>