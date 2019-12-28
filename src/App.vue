<template>
    <div id="app">
        <Header
            :totalQtnNumber="questions.length"
            :numberCorrectAns="numberCorrectAns"
        />
        <QuestionBox
            v-if="questions.length"
            :currentQtn="questions[index]"
            :qtnNum="index"
            :next="next"
            :increment-correct-answer-counter="incrementCorrectAnswerCounter"
        />
    </div>
</template>

<script>
    import QuestionBox from './components/QuestionBox'
    import Header from './components/Header'

    export default {
        name: 'app',
        components: {
            QuestionBox, Header
        },
        data() {
            return {
                questions: [],
                index: 0,
                numberCorrectAns : 0
            }
        },
        methods: {
            next() {
                this.index++;
            },
            incrementCorrectAnswerCounter(isCorrect) {
                if (isCorrect) {
                    this.numberCorrectAns++
                }
            }
        },
        mounted() {
            this.axios.get('https://opentdb.com/api.php?amount=10&category=9&type=multiple').then((response) => {
                this.questions = response.data.results;
            });
        }
    }
</script>

<style lang="scss">
    @import "./scss/global.scss";
</style>
