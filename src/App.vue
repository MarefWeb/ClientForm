<template>
  <div id="app">
    <FirstStep v-if="step === 1" @next-step="firstStep" />
    <SecondStep v-if="step === 2" @previous-step="previousStep" @next-step="secondStep" />
    <ThirdStep v-if="step === 3" @previous-step="previousStep" @finish="finish" />
  </div>
</template>

<script>
import FirstStep from '@/components/FirstStep'
import SecondStep from '@/components/SecondStep'
import ThirdStep from '@/components/ThirdStep'

export default {
  name: 'App',

  data() {
    return {
      step: 1,
      finished: false,
      userData: {
        firstStep: {},
        secondStep: {},
        thirdStep: {}
      }
    }
  },

  methods: {
    nextStep() {
      this.step < 3 ? this.step++ : false
    },

    previousStep() {
      this.step > 1 ? this.step-- : false
    },

    firstStep(data) {
      this.userData.firstStep = data
      this.nextStep()
    },

    secondStep(data) {
      this.userData.secondStep = data
      this.nextStep()
    },


    finish(data) {
      if(this.finished == false) {
        this.userData.thirdStep = data

        console.log(this.userData)

        this.finished = true
      }
    }
  },

  components: {
    FirstStep,
    SecondStep,
    ThirdStep
  }

}
</script>

<style lang="sass">

form
	display: flex
	flex-direction: column
	align-items: center

button
	cursor: pointer
	outline: none
	background-color: white
	border: 1px solid gray
	border-radius: 10px
	padding: 7px

p
	margin: 8px 0

select 
	outline: none
	border: 1px solid gray
	border-radius: 5px

select[multiple]
	overflow-y: auto

select
	display: block

.form-row 
  display: flex
  justify-content: center
  align-items: center
  flex-wrap: wrap
  margin: 5px 0

  & span 
    font-size: 14px

  & input
    margin: 5px 0
    margin-left: 10px

  & select
    margin: 0 auto

  & p
    text-align: center

.rounded-field
	outline: none
	border: 1px solid gray
	border-radius: 15px

	height: 30px
	width: 200px

	padding-left: 15px

.btn-back
  margin-right: 10px

.input-error
  border: 1px solid red

.validation-error
	color: red
	font-size: 13px

.validation-ok
  text-align: center
  color: green
  font-size: 13px

</style>
