<script setup lang="ts">
import { ref } from 'vue';


interface FormData {
  username: string;
  role: number | null;
  email: string;
  password: string;
  password_repeat: string;
  agree: boolean;
}

const formData = ref<FormData>({
  username: '',
  role: null,
  email: '',
  password: '',
  password_repeat: '',
  agree: true // по умолчанию согласие включено
});

const roles = [
  { value: null, name: 'Должность' },
  { value: 1, name: 'Должность 1' },
  { value: 2, name: 'Должность 2' },
  { value: 3, name: 'Должность 3' }
];

const errors = ref({
  username: '',
  email: '',
  password: '',
  password_repeat: ''
});

const registrationSuccess = ref(false);

const submitForm = () => {
  // Проверка на заполнение всех обязательных полей
  if (!formData.value.username) errors.value.username = 'Введите имя пользователя';
  else errors.value.username = '';

  if (!formData.value.email) errors.value.email = 'Введите email';
  else errors.value.email = '';

  if (!formData.value.password) errors.value.password = 'Введите пароль';
  else errors.value.password = '';

  if (!formData.value.password_repeat) errors.value.password_repeat = 'Повторите пароль';
  else errors.value.password_repeat = '';

  if (formData.value.password !== formData.value.password_repeat) {
    errors.value.password = 'Пароли не совпадают';
    errors.value.password_repeat = 'Пароли не совпадают';
  }

   // Проверка согласия на обработку персональных данных
   if (!formData.value.agree) {
    alert('Необходимо согласие на обработку персональных данных');
    return; // Прерываем отправку формы
  }

  // Если все поля заполнены, отправляем данные
  if (!Object.values(errors.value).some(error => error !== '')) {
    // Моковый POST запрос
    console.log('Отправка данных:', formData.value);
    // Мой код отправки данных здесь
    registrationSuccess.value = true;
  }
};
</script>

<template>
 
    <div class="wrapper">
     
      <div class="registration">
        <div>
          <h1 class="registration__title">Регистрация</h1>
          <p v-if="!registrationSuccess" class="registration__subtitle">Заполните Ваши данные</p>
        </div>
  
        <form v-if="!registrationSuccess" @submit.prevent="submitForm" class="registration-form">
         
          <div class="form-group">
            
            <div class="form-group__error">
            <input 
            class="form-group__input" 
            type="text" id="username" 
            v-model.trim="formData.username"
            placeholder="Имя"
            >
            <div v-if="errors.username" class="error-message">{{ errors.username }}</div>
            </div>

            <div class="form-group__error">
              <input 
              class="form-group__input" 
              type="email" id="email" 
              v-model.trim="formData.email"
              placeholder="Email"
              >
  
              <div v-if="errors.email" class="error-message">{{ errors.email }}</div>
            </div>
           
          </div>
          
    
         <div class="form-group">

          <input type="text" class="form-group__input form-group__none">

          <select 
          class="form-group__input form-group__select" 
          id="role" 
          v-model="formData.role"
          >
            <option 
            class="form-group__option"
            v-for="option in roles" 
            :key="option.value" 
            :value="option.value"
            >
            {{ option.name }}
          </option>
          </select>
         </div>
    
      
          <div class="form-group">
            
            <div class="form-group__error">
              <input 
            class="form-group__input" 
            type="password" 
            id="password" 
            v-model.trim="formData.password"
            placeholder="Пароль"
            >
            <div v-if="errors.password" class="error-message">{{ errors.password }}</div>
            </div>

            <div class="form-group__error">
              <input 
          class="form-group__input" 
          type="password" 
          id="password_repeat" 
          v-model.trim="formData.password_repeat"
          placeholder="Повторите пароль"
          >
          <div v-if="errors.password_repeat" class="error-message">{{ errors.password_repeat }}</div>
            </div>
          </div>
          
    
         <div class="form-group__show">
          
          <label class="checkbox-container">
            <input type="checkbox">
            <span class="checkmark"></span>
          </label>

          <div>
            <h4>Хотите чтобы ваш профиль видели другие участники платформы?</h4>
            <p>Включает профиль для просмотра другими пользователями по ссылке</p>
          </div>

         </div>
    
          <div class="form-group__check">
            <div class="form-group__check-item">
              <input class="form-group__checkbox" type="checkbox" id="agree" v-model="formData.agree">
              <label for="agree">Регистрируясь, Вы соглашаетесь  с <a href="#" class="form-group__link">политикой конфиденциальности</a> и обработкой <a href="#" class="form-group__link">персональных данных</a></label>
            </div>
            <button class="form-group__button" type="submit">Зарегистрироваться</button>
          </div>
        </form>
    
        <div v-if="registrationSuccess" class="success-message">
          <span>Регистрация успешно завершена успешно!)</span>
          <img src="data:image/jpeg;base64,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" alt="">
        </div>
      </div>
     
    </div>
  
</template>



<style scoped lang="scss">

</style>
