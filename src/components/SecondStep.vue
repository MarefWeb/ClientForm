<template>
  <form novalidate>
      <div class="form-row">
        <div>
          <input type="text" class="rounded-field" placeholder="Введите название страны" v-model.trim="country">
          <p class="validation-error" v-if="!$v.country.alpha">Поле должно содержать только буквы</p>
        </div>

        <div>
          <input type="text" class="rounded-field" placeholder="Введите название области" v-model.trim="region">
          <p class="validation-error" v-if="!$v.region.alpha">Поле должно содержать только буквы</p>
        </div>
      </div>

      <div class="form-row">
        <div>
          <input type="text" class="rounded-field" placeholder="Введите название города*" :class="{ 'input-error': $v.city.$error }" v-model.trim="city">
          <p class="validation-error" v-if="!$v.city.alpha">Поле должно содержать только буквы</p>
          <p class="validation-error" v-else-if="$v.city.$error">Поле обязательно для заполнения</p>
        </div> 

        <div>
          <input type="text" class="rounded-field" placeholder="Введите индекс" v-model.trim="index">
          <p class="validation-error" v-if="!$v.index.numeric">Поле должно содержать только цыфры</p>
        </div>
      </div>

      <div class="form-row">
        <div>
          <input type="text" class="rounded-field" placeholder="Введите название улицы" v-model.trim="street">
          <p class="validation-error" v-if="!$v.street.alpha">Поле должно содержать только буквы</p>
        </div>

        <div>
          <input type="text" class="rounded-field" placeholder="Введите номер дома" v-model.trim="house">
          <p class="validation-error" v-if="!$v.house.numeric">Поле должно содержать только цыфры</p>
        </div>
      </div>

      <div class="form-row">
        <button type="button" class="btn-back" @click="$emit('previous-step')">Назад</button>
        <button type="button" @click="nextStep">Следующий шаг</button>
      </div>
      <p class="validation-error" v-if="submitStatus == 'ERROR'">Пожалуйста, заполните форму правильно.</p>
  </form>
</template>

<script>
import { required, numeric } from 'vuelidate/lib/validators'


export default {
  
  data() {
    return {
      submitStatus: '',
      country: '',
      region: '',
      city: '',
      index: '',
      street: '',
      house: ''
    }
  },

	methods: {
		nextStep() {
			this.$v.$touch()

      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
			}

			else {
				this.$emit('next-step', {country: this.country, region: this.region, city: this.city, index: this.index, street: this.street, house: this.house})
			}
		}
	},

	validations: {
    city: { 
      required,
      alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
    },

    country: {
      alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
    },

    region: {
      alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
    },

    index: {
      numeric
    },

    street: {
      alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
    },

    house: {
      numeric
    }
  }

}
</script>
