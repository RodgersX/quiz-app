<template>
    <div class="question-box">
        <div class="wrapper">
            <h3>
                {{ currentQuestion.question }}
            </h3>

            <ol type="i">
                <li 
                v-for="(answer, index) in answers" 
                :key="index" 
                @click="selectAnswer(index)"
                :class="answerClass(index)">{{answer}}</li>
            </ol>

            <button 
            @click="submitAnswer" 
            :disabled="selectedIndex === null || answered">Submit</button>
            <button @click="next">next</button>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'

export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },

    data() {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false,


        }
    },

    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },

    //keeps track of the changes in the props
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        },
    },

    methods: {
        selectAnswer(index) {
            this.selectedIndex = index    
        },
        shuffleAnswers() {
             let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
             this.shuffledAnswers = _.shuffle(answers)
             this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer() {
            let isCorrect = false
            if(this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        answerClass(index) {
            let answerClass = ''
            if(!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if(this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if(this.answered && this.selectedIndex ===index && this.correctIndex !== index) {
                answerClass = 'incorrect'
            }

            return answerClass
        }
    }
}
</script>

<style scoped>
li:hover {
    background: #EEE;
}

.selected {
    background: lightblue;
}

.correct {
    background: lightgreen;
}

.incorrect {
    background: red;
}
</style>