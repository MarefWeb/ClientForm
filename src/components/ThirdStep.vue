<template>
	<form novalidate>
		<div class="form-row">
			<div>
				<p>Выберете тип документа*</p>
				<p class="validation-error" v-if="$v.documentType.$error">Нужно выбрать тип документа</p>
				<select :class="{ 'input-error': $v.documentType.$error }" v-model.trim="documentType">
					<option value="Паспорт">Паспорт</option>
					<option value="Свидетельство о рождении">Свидетельство о рождении</option>
					<option value="Водительское удостоверение">Водительское удостоверение</option>
				</select>
			</div>
		</div>

		<div class="form-row">
			<div>
				<p>Выберете дату выдачи документа*</p>
				<p class="validation-error" v-if="$v.documentIssuedDate.$error">Нужно выбрать дату выдачи документа</p>
				<input type="date" class="rounded-field" :class="{ 'input-error': $v.documentIssuedDate.$error }" v-model.trim="documentIssuedDate">
			</div>
		</div>

		<div class="form-row">
			<div>
				<input type="text" class="rounded-field" placeholder="Серия документа" v-model.trim="documentSeries">
				<p class="validation-error" v-if="!$v.documentSeries.numeric">Поле должно содержать только цыфры</p>
			</div>
		</div>

		<div class="form-row">
			<div>
				<input type="text" class="rounded-field" placeholder="Номер документа" v-model.trim="documentNumber">
				<p class="validation-error" v-if="!$v.documentNumber.numeric">Поле должно содержать только цыфры</p>
			</div>
		</div>

		<div class="form-row">
			<div>
				<input type="text" class="rounded-field" placeholder="Кем был выдан документ" v-model.trim="personIssuedDocument">
				<p class="validation-error" v-if="!$v.personIssuedDocument.alpha">Поле должно содержать только буквы</p>
			</div>
		</div>

		<div class="form-row">
			<button type="button" class="btn-back" @click="$emit('previous-step')">Назад</button>
			<button type="button" @click="nextStep">Завершить</button>
		</div>
		<p class="validation-error" v-if="submitStatus == 'ERROR'">Пожалуйста, заполните форму правильно.</p>
		<p class="validation-ok" v-if="submitStatus == 'OK'">Регистрация прошла успешно!<br>(можете посмотреть свои данные в консоли разработчика)</p>
	</form>
</template>

<script>
import { required, numeric } from 'vuelidate/lib/validators'

export default {

	data() {
		return {
			submitStatus: '',
			documentType: '',
			documentSeries: '',
			documentNumber: '',
			personIssuedDocument: '',
			documentIssuedDate: ''
		}
	},

	methods: {
		nextStep() {
			this.$v.$touch()

      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
			}

			else {
				this.$emit('finish', {documentType: this.documentType, documentSeries: this.documentSeries, documentNumber: this.documentNumber,
				personIssuedDocument: this.personIssuedDocument, documentIssuedDate: this.documentIssuedDate})
				this.submitStatus = 'OK'
			}
		}
	},

	validations: {
		documentType: { 
			required 
		},

		documentIssuedDate: { 
			required 
		},

		documentSeries: {
			numeric
		},

		documentNumber: {
			numeric
		},

		personIssuedDocument: {
			alpha: val => /^[а-яёА-ЯЁA-Zaz]*$/i.test(val)
		}
  }

}
</script>
