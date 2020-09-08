<template>
	<form novalidate>
		<div class="form-row">
			<div>
				<input type="text" placeholder="Введите ваше имя*" class="rounded-field" :class="{ 'input-error': $v.name.$error }" v-model.trim="name">
				<p class="validation-error" v-if="!$v.name.alpha">Поле должно содержать только буквы</p>
				<p class="validation-error" v-else-if="$v.name.$error">Поле обязательно для заполнения</p>
			</div>

			<div>
				<input type="text" placeholder="Введите вашу фамилию*" class="rounded-field" :class="{ 'input-error': $v.surname.$error }" v-model.trim="surname">
				<p class="validation-error" v-if="!$v.surname.alpha">Поле должно содержать только буквы</p>
				<p class="validation-error" v-else-if="$v.surname.$error">Поле обязательно для заполнения</p>
			</div>
		</div>

		<div class="form-row">
			<div>
				<input type="text" placeholder="Введите ваше отчество" class="rounded-field" :class="{ 'input-error': $v.patronymic.$error }" v-model.trim="patronymic">
				<p class="validation-error" v-if="!$v.patronymic.alpha">Поле должно содержать только буквы</p>
			</div>

			<div>
				<input type="tel" maxlength="11" id="tel" placeholder="Введите ваш номер телефона*" class="rounded-field" :class="{ 'input-error': $v.phone.$error }" v-model.trim="phone">
				<p class="validation-error" v-if="!$v.phone.startBySeven">Номер должен начинатся с 7</p>
				<p class="validation-error" v-else-if="!$v.phone.numeric">Поле должно содержать только цыфры</p>
				<p class="validation-error" v-else-if="$v.phone.$error">Поле обязательно для заполнения</p>
			</div>
		</div>
		
		<div class="form-row">
			<div>
				<p>Выберете вашу дату рожения*</p>
				<p class="validation-error" v-if="$v.bDate.$error">Поле обязательно для заполнения</p>
				<input type="date" class="rounded-field" :class="{ 'input-error': $v.bDate.$error }" v-model.trim="bDate">
			</div>
		</div>

		<div class="form-row">
			<label for="man">
				<input type="radio" id="man" value="Мужчина" name="sex" v-model.trim="sex">
				Мужчина
			</label>

			<label for="wooman">
				<input type="radio" id="wooman" value="Женщина" name="sex" v-model.trim="sex"> 
				Женщина
			</label>
		</div>

		<div class="form-row">
			<div>
				<p>Выберете вашу группу<br>(для выбора нескольких значений используйте shift и ctrl)*</p>
				<p class="validation-error" v-if="$v.bDate.$error">Поле обязательно для заполнения</p>
				<select multiple="multiple" :class="{ 'input-error': $v.clientGroup.$error }" v-model.trim="clientGroup">
					<option value="VIP">VIP</option>
					<option value="Проблемные">Проблемные</option>
					<option value="ОМС">ОМС</option>
				</select>
			</div>
		</div>

		<div class="form-row">
			<div>
				<p>Выберете вашего лечещего врача</p>
				<select v-model.trim="doctor">
					<option value="Иванов">Иванов</option>
					<option value="Захаров">Захаров</option>
					<option value="Чернышова">Чернышова</option>
				</select>
			</div>
		</div>

		<div class="form-row">
			<span>Не отсылать сообщения</span>
			<input type="checkbox" v-model.trim="noSendMessages">
		</div>

		<button type="button" @click="nextStep">Следующий шаг</button>
		<p class="validation-error" v-if="submitStatus == 'ERROR'">Пожалуйста, заполните форму правильно.</p>
	</form>
</template>

<script>
import { required, minLength, numeric } from 'vuelidate/lib/validators'

export default {
	
	data() {
		return {
			submitStatus: '',
			name: '',
			surname: '',
			patronymic: '',
			phone: '7',
			bDate: '',
			sex: '',
			clientGroup: [],
			doctor: '',
			noSendMessages: ''
		}
	},

	methods: {
		nextStep() {
			this.$v.$touch()

      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
			}

			else {
				this.$emit('next-step', {name: this.name, surname: this.surname, patronymic: this.patronymic, phone: this.phone, bDate: this.bDate, 
				sex: this.sex, clientGroup: this.clientGroup, doctor: this.doctor, noSendMessages: this.noSendMessages})
			}
		}
	},

	validations: {
		name: {
			required,
			alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
		},

		surname: { 
			required,
			alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
		},

		patronymic: {
			alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
		},

		phone: { 
			required,
			minLength: minLength(11) ,
			numeric,
			startBySeven: val => /^7/i.test(val)
		},

		bDate: { required },
		clientGroup: { required }
	}

}
</script>