<template>
    <div>
        <div class="container mt-3">
            <div class="row">
                <div class="col-lg-6 col-md-8 mx-auto">
                    <b-jumbotron>
                        <template v-slot:header>Question {{ incrementCounter() }}.</template>

                        <p>{{ currentQtn.question }}</p>
                        <hr class="my-4">

                        <div v-for="(questionOption, index) in questionOptions" :key="index">
                            <b-form-radio
                                v-model="selectedIndex"
                                :value="index"
                                @change="selectedAnswer(index)"
                                :class="answerClass(index)"
                            >
                                {{ questionOption }}
                            </b-form-radio>
                        </div>

                        <hr class="my-4">

                        <b-button
                                variant="info" href="#" class="mr-3" @click="submitAnswer"
                                :disabled="selectedIndex === null || answered === true">
                            Submit
                        </b-button>
                        <b-button variant="success" href="#" @click="next" :disabled="!answered === true">Next</b-button>
                    </b-jumbotron>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'QuestionBox',
        props: {
            currentQtn: Object,
            qtnNum: Number,
            next: Function,
            incrementCorrectAnswerCounter: Function
        },
        data() {
            return {
                selectedIndex: null,
                correctIndex: null,
                answered: false,
            }
        },
        computed: {
            questionOptions() {
                let options = [...this.currentQtn.incorrect_answers];
                options.push(this.currentQtn.correct_answer);
                return this.shuffleAnswer(options)
            }
        },
        watch: {
            currentQtn: {
                immediate: true,
                handler() {
                    let options = [...this.currentQtn.incorrect_answers, this.currentQtn.correct_answer];
                    this.selectedIndex = null;
                    this.answered = false;
                    this.correctIndex = this.shuffleAnswer(options).indexOf(this.currentQtn.correct_answer)
                }
            }
        },
        methods: {
            incrementCounter() {
                return this.qtnNum + 1;
            },
            selectedAnswer(index) {
                this.selectedIndex = index;
            },
            shuffleAnswer(array) {
                let options = array;
                let counter = options.length;
                while (counter > 0) {
                    // Pick a random index
                    let index = Math.floor(Math.random() * counter);
                    counter--;
                    // And swap the last element with it
                    let temp = options[counter];
                    options[counter] = options[index];
                    options[index] = temp;
                }
                return options;
            },
            submitAnswer() {
                let isCorrect = false;
                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true;
                }
                this.answered = true;
                this.incrementCorrectAnswerCounter(isCorrect);
            },
            answerClass(index) {
                let className = '';
                if (!this.answered && this.selectedIndex === index ) {
                    className = 'selected';
                } else if (this.answered && this.correctIndex === index) {
                    className = 'correct';
                } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                    className = 'incorrect';
                }
                return className;
            }
        },
    }
</script>