<template>
	<div class="app">
		<form @submit.prevent="submitForm">
			<h1>Анкета пользователя 
				<span class="msg" 
					:class="{success : v$.firstName.$dirty && !v$.firstName.$error && v$.secondName.$dirty && !v$.secondName.$error && v$.thirdName.$dirty && !v$.thirdName.$error && v$.email.$dirty && !v$.email.$error && v$.age.$dirty && !v$.age.$error && v$.password.$dirty && !v$.password.$error && v$.confirmPassword.$dirty && !v$.confirmPassword.$error && v$.sobaka.$dirty && !v$.sobaka.$error && v$.ipaddress.$dirty && !v$.ipaddress.$error && v$.alphanum.$dirty && !v$.alphanum.$error}">Успешно!</span>
				<span class="msg"
					:class="{error : v$.firstName.$error || v$.secondName.$error || v$.thirdName.$error || v$.email.$error || v$.age.$error || v$.password.$error || v$.confirmPassword.$error || v$.sobaka.$error || v$.ipaddress.$error || v$.alphanum.$error}">Имеются ошибки!</span>
			</h1>
			<p style="font-size: 14px; margin-bottom: 30px; color: rosybrown;">Все поля обязательны для заполнения!</p>
			<my-input
				label="Имя"
				name="name"
				placeholder="Введите ваше Имя"
				:class="{error: v$.firstName.$error, done : v$.firstName.$dirty && !v$.firstName.$error}"
				v-model:value="v$.firstName.$model"
				:error="v$.firstName.$errors"
			></my-input>
			<my-input
				label="Фамилия"
				name="surnme"
				placeholder="Введите вашу Фамилию"
				:class="{error: v$.secondName.$error, done : v$.secondName.$dirty && !v$.secondName.$error}"
				v-model:value="v$.secondName.$model"
				:error="v$.secondName.$errors"
			></my-input>
			<my-input
				label="Отчество"
				name="third"
				placeholder="Введите ваше Отчество"
				:class="{error: v$.thirdName.$error, done : v$.thirdName.$dirty && !v$.thirdName.$error}"
				v-model:value="v$.thirdName.$model"
				:error="v$.thirdName.$errors"
			></my-input>
			<my-input
				label="Email"
				name="email"
				placeholder="Введите ваш Email"
				:class="{error: v$.email.$error, done : v$.email.$dirty && !v$.email.$error}"
				v-model:value="v$.email.$model"
				:error="v$.email.$errors"
			></my-input>
			<my-input
				label="Возраст"
				name="age"
				placeholder="Введите ваш возраст (от 1 до 99)"
				:class="{error: v$.age.$error, done : v$.age.$dirty && !v$.age.$error}"
				v-model:value="v$.age.$model"
				:error="v$.age.$errors"
			></my-input>
			<my-input
				label="Ваш Пароль"
				name="password"
				type="password"
				placeholder="Введите пароль"
				:class="{error: v$.password.$error, done : v$.password.$dirty && !v$.password.$error}"
				v-model:value="v$.password.$model"
				:error="v$.password.$errors"
			></my-input>
			<my-input
				label="Повтор пароля"
				name="confirm"
				type="password"
				placeholder="Повторите пароль"
				:class="{error: v$.confirmPassword.$error, done : v$.confirmPassword.$dirty && !v$.confirmPassword.$error}"
				v-model:value="v$.confirmPassword.$model"
				:error="v$.confirmPassword.$errors"
			></my-input>
			<my-input
				label="собака"
				name="sobaka"
				placeholder="собака"
				v-model:value="v$.sobaka.$model"
				:class="{error: v$.sobaka.$error, done : v$.sobaka.$dirty && !v$.sobaka.$error}"
				:error="v$.sobaka.$errors"
			></my-input>
			<my-input
				label="IP адрес"
				name="ipaddress"
				placeholder="Ваш IP адрес"
				v-model:value="v$.ipaddress.$model"
				:class="{error: v$.ipaddress.$error, done : v$.ipaddress.$dirty && !v$.ipaddress.$error}"
				:error="v$.ipaddress.$errors"
			></my-input>
			<my-input
				label="Цифры или буквы"
				name="alphanum"
				placeholder="Цифры или буквы"
				v-model:value="v$.alphanum.$model"
				:class="{error: v$.alphanum.$error, done : v$.alphanum.$dirty && !v$.alphanum.$error}"
				:error="v$.alphanum.$errors"
			></my-input>
		</form>
	</div>
</template>

<script>
import { useVuelidate } from '@vuelidate/core'
import {  helpers, required, email, minLength, alpha, numeric, alphaNum, sameAs, between, ipAddress} from '@vuelidate/validators'
const isSobaka = (value) => value.includes('собака')
const submitForm = () => {
	v$.value.$touch();
	console.log('v');
}
  	export default {
		setup() {
			return { v$: useVuelidate() }
		},
		data() {
			return {
				firstName: '',
				secondName: '',
				thirdName: '',
				email: '',
				age: '',
				password: '',
				confirmPassword: '',
				sobaka: '',
				ipaddress: '',
				alphanum: '',
			}
		},
		validations() {
			return {
				firstName: { 
					minLength: helpers.withMessage('Минимальная длина: 3 символа!', minLength(2)),
				 	required: helpers.withMessage('Заполните поле!', required),
					alpha: helpers.withMessage('Только буквы!', alpha)
				},
				secondName: { 
					minLength: helpers.withMessage('Минимальная длина: 3 символа!', minLength(2)),
					required: helpers.withMessage('Заполните поле!', required),
					alpha: helpers.withMessage('Только буквы!', alpha)
				},
				thirdName: { 
					minLength: helpers.withMessage('Минимальная длина: 3 символа!', minLength(3)),
					alpha: helpers.withMessage('Только буквы!', alpha),
					required: helpers.withMessage('Заполните поле!', required),
				},
				email: { 
					email: helpers.withMessage('Это не email!', email),
					required: helpers.withMessage('Заполните поле!', required),
				},
				age: { 
					between: helpers.withMessage('Не врите!', between(1,99)),
					required: helpers.withMessage('Заполните поле!', required),
					numeric: helpers.withMessage('Только числа!', numeric)
				},
				password: {
					required: helpers.withMessage('Заполните поле!', required),
				},
				confirmPassword: {
					sameAsPassword: helpers.withMessage('Пароли не совпадают!', sameAs(this.password)),
					required: helpers.withMessage('Заполните поле!', required),
				},
				sobaka: {
					sobaka: helpers.withMessage('Тут нет собаки', isSobaka),
					required: helpers.withMessage('Заполните поле!', required),
				},
				ipaddress: {
					ipAddress: helpers.withMessage('Это не IP адрес!', ipAddress),
					required: helpers.withMessage('Заполните поле!', required),
				},
				alphanum: {
					alphaNum: helpers.withMessage('Только цифры или буквы!', alphaNum),
					required: helpers.withMessage('Заполните поле!', required),
				}
			}
		}
  	}
</script>

<style scoped>
	* {
		padding: 0;
		margin: 0;
		box-sizing: border-box;
		font-size: 20px;
	}
	.app {
		background-color: white;
		font-family: 'Roboto';
		padding: 30px;
	}
	.app h1 {
		display: flex;
		align-items: center;
		justify-content: space-between;
	}
	.msg {
		font-size: 14px;
		padding: 10px;
		border-radius: 20px;
		color: white;
	}
	.error {
		box-shadow: 0 0 10px 0 rgb(209, 42, 0, .6);
		background-color: rgb(209, 42, 0);
	}
	.success {
		background: rgb(14, 193, 14);
    	box-shadow: 0 0 10px 0 rgb(4, 255, 4, .5);
	}
	form {
		box-shadow: 0 0 15	px 0 rgba(0,0,0, .3);
		background-color: white;
		border: 2px solid rgb(255, 105, 0);
		border-radius: 20px;
		width: 600px;
		margin: 0 auto;
		padding: 50px 30px;
	}
</style>