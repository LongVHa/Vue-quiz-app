<template>
    <div class="question-box-container">
        <b-jumbotron header="" lead="Bootstrap v4 Components for Vue.js 2">
            <template v-slot:lead>
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">
            
            <b-list-group>
                <b-list-group-item 
                    v-for="(answer, index) in shuffledAnswers" 
                    :key="index"
                    @click="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                     {{answer}}
                </b-list-group-item>
            </b-list-group>

            <b-button 
                variant="primary" 
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
            Submit
            </b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
        </b-jumbotron>
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
    data(){
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false        

        }
    },
    computed: {
        answers(){
            // let answers = [...this.currentQuestion.incorrect_answers]
            // answers.push(this.currentQuestion.correct_answer)

            return this.shuffledAnswers
        }
    },
    watch: {
        //watches props
        //shuffle answers

        currentQuestion: { //here we use an object
            immediate: true,
            handler(){
               this.selectedIndex = null,
               this.answered = false,
               this.shuffleAnswers()     
            }
        }

        // currentQuestion(){ //here we use a function
        //     this.selectedIndex = null
        //     this.shuffleAnswers()
        // }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index
            console.log(index)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        submitAnswer(){
            let isCorrect = false
            //console.log('selected-', this.selectedIndex, 'correct-', this.correctIndex)
            if(this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        answerClass(index){
            let answerClass = ''

            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected'
            }else if(this.answered && this.correctIndex ===  index){
                answerClass = 'correct'
            }else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
                answerClass = 'incorrect'
            }

            return answerClass
        }
    }
}
</script>

<style scoped>
    .list-group{
        margin-bottom:15px;
    }
    .list-group-item:hover{
        background: #eee;
        cursor: pointer;
    }
    .btn{
        margin:0 5px;
    }
    .selected{
        background-color:lightblue;
    }
    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color: red;
    }
</style>