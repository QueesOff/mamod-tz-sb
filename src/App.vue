<template>
  <div class="container">
    <form @submit.prevent="submitForm" v-if="!registrationSuccess">
      <h1>Регистрация</h1>
      <hr />
      <p>Заполните Ваши данные</p>
      <div class="grid-container">
        <div class="input-container">
          <input
            placeholder="Имя"
            type="text"
            id="username"
            v-model="formData.username"
            :class="{ error: formErrors.username }"
            required
          />
          <span v-if="formErrors.username" class="error-message"
            >Поле обязательно для заполнения</span
          >
        </div>
        <div class="input-container">
          <input
            placeholder="Email"
            type="email"
            id="email"
            v-model="formData.email"
            :class="{ error: formErrors.email }"
            required
          />
          <span v-if="formErrors.email" class="error-message"
            >Поле обязательно для заполнения</span
          >
        </div>
        <div class="input-container">
          <select
            id="role"
            v-model="formData.role"
            :class="{ error: formErrors.role }"
            required
          >
            <option :value="null" disabled class="sp">Должность</option>
            <option
              v-for="option in positions"
              :key="option.value"
              :value="option.value"
            >
              {{ option.name }}
            </option>
          </select>
          <span v-if="formErrors.role" class="error-message"
            >Поле обязательно для заполнения</span
          >
        </div>
        <div class="input-container">
          <input
            placeholder="Пароль"
            type="password"
            id="password"
            v-model="formData.password"
            :class="{ error: formErrors.password }"
            required
          />
          <span
            class="password-toggle"
            @click="togglePasswordVisibility('password')"
          >
            <img
              :src="
                passwordVisible
                  ? require('@/assets/eye-close.svg')
                  : require('@/assets/eye.svg')
              "
              alt="Показать пароль"
            />
          </span>

          <span v-if="formErrors.password" class="error-message"
            >Поле обязательно для заполнения</span
          >
        </div>

        <div class="input-container">
          <input
            placeholder="Повторите пароль"
            type="password"
            id="password_repeat"
            v-model="formData.password_repeat"
            :class="{ error: formErrors.password_repeat }"
            required
          />
          <span
            class="password-toggle"
            @click="togglePasswordVisibility('password_repeat')"
          >
            <img
              :src="
                passwordRepeatVisible
                  ? require('@/assets/eye-close.svg')
                  : require('@/assets/eye.svg')
              "
              alt="Показать пароль"
            />
          </span>

          <span v-if="formErrors.password_repeat" class="error-message"
            >Поле обязательно для заполнения</span
          >
        </div>
      </div>
      <hr />
      <div class="checkbox">
        <input type="checkbox" class="switch" />
        <div class="text">
          <h4>Хотите чтобы Ваш профиль видели другие участники платформы?</h4>
          <p>Включает профиль для просмотра другими пользователями по ссылке</p>
        </div>
      </div>
      <div class="end">
        <div>
          <input type="checkbox" class="ui-checkbox" />
          <p>
            Регистрируясь, Вы соглашаетесь с
            <a href="#"> политикой конфиденциальности</a> <br />
            и обработкой <a href="#">персональных данных</a>
          </p>
        </div>
        <button type="submit">Зарегистрироваться</button>
      </div>
    </form>
    <p v-else>Регистрация успешно завершена</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        username: "",
        role: null,
        email: "",
        password: "",
        password_repeat: "",
        agreement: true,
      },
      positions: [
        { value: 1, name: "Разработчик" },
        { value: 2, name: "Дизайнер" },
        { value: 3, name: "Менеджер" },
      ],
      formErrors: {},
      registrationSuccess: false,
      passwordVisible: false,
      passwordRepeatVisible: false,
    };
  },
  methods: {
    togglePasswordVisibility(inputId) {
      const input = document.getElementById(inputId);
      if (input) {
        this[inputId + "Visible"] = !this[inputId + "Visible"];
        input.type = this[inputId + "Visible"] ? "text" : "password";
      }
    },
    submitForm() {
      this.validateForm();
      if (
        Object.keys(this.formErrors).length === 0 &&
        this.formData.agreement
      ) {
        // Моковый POST запрос
        console.log("Отправка данных:", this.formData);
        this.registrationSuccess = true;
      }
    },
    validateForm() {
      this.formErrors = {};
      for (const key in this.formData) {
        if (!this.formData[key]) {
          this.$set(this.formErrors, key, true);
        }
      }
    },
  },
};
</script>

<style>
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}
form {
  background-color: white;

  padding: 30px;
  border: 1px solid rgba(230, 230, 235, 1);
  border-radius: 10px;
}
.grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 40px;
}

.input-container:nth-child(1) {
  grid-column: 1;
}

.input-container:nth-child(2) {
  grid-column: 2;
}

.input-container:nth-child(3) {
  grid-column: 2;
}

.input-container:nth-child(4) {
  grid-column: 1;
}

.input-container:nth-child(5) {
  grid-column: 2;
}

.input-container:nth-child(6) {
  grid-column: 2;
}

.input-container {
  display: flex;
  flex-direction: column;
}
input,
select {
  width: 450px;
  border: 1px solid rgba(230, 230, 235, 1);
  border-radius: 11px;
}
select {
  width: 472px;
  padding: 10px 0 10px 10px;
}
option,
input,
input::placeholder {
  padding: 10px;
  color: rgb(0, 0, 0);
}

hr {
  margin: 30px 0;
}
.checkbox {
  /* margin-top: 40px; */
  display: flex;
  text-align: center;
  align-items: start;
  grid-column: span 2;
}

.text {
  padding: 5px;
}
.text p {
  text-align: start;
  font-size: 14px;
  color: rgba(105, 105, 119, 1);
}
.end {
  display: flex;
  justify-content: space-between;
}
.end div {
  font-size: 14px;
  display: flex;
  align-items: center;
  gap: 10px;
}
a {
  text-decoration: none;
  color: rgba(53, 134, 255, 1);
}
h1,
h4 {
  margin: 0;
}
button {
  width: 302px;
  height: 40px;
  background-color: rgba(73, 122, 218, 0.2);
  color: rgba(73, 122, 218, 1);
  border: 1px solid rgba(73, 122, 218, 0.2);
  border-radius: 8px;
  transition: 0.3s;
}
button:hover {
  scale: 1.05;
}
.error {
  border: 1px solid red;
}
.error-message {
  color: red;
}
</style>
