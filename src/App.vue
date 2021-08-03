<template>
  <div class="container form__container">
    <div class="form-wrapper">
      <transition name="fade" mode="out-in">
        <form class="form" v-if="!formDone" @submit.prevent="sendForm">
          <h2 class="form__title">Регистрация</h2>
          <p class="form__descr">Уже есть аккаунт? <a href="#">Войти</a></p>
          <input type="hidden" value="Форма регистрации" />

          <AppLabel
            v-for="(label, i) in info"
            :key="i"
            :name="label.name"
            :value="label.value"
            :type="label.type"
            :valid="label.valid"
            :placeholder="label.placeholder"
            @updated="onInput(i, $event)"
          />

          
          <AppCustomSelect
            :options="options"
            :selected="language"
            @selected="optionSelected"
          />
          

          <label class="custom-checkbox form-checkbox">
            <input
              type="checkbox"
              class="custom-checkbox__field"
              v-model="confirm"
            />
            <span class="custom-checkbox__content"
              >Принимаю <a href="#">условия</a> использования</span
            >
          </label>

          <button class="form__btn" :disabled="!formReady">
            Зарегистрироваться
          </button>
        </form>
        <div class="form-done" v-else>
          <table class="table">
            <tbody>
              <tr v-for="(field, i) in info" :key="i">
                <td>{{ field.name }}:</td>
                <td>{{ field.value }}</td>
              </tr>
              <tr>
                <td>Язык:</td>
                <td>{{ language }}</td>
              </tr>
              <tr>
                <td>Ознакомлен с условиями:</td>
                <td>{{ confirm ? 'Да' : 'Нет' }}</td>
              </tr>
            </tbody>
          </table>
          <button class="form__btn" @click="backToForm">
            Вернуться
          </button>
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import AppCustomSelect from './components/AppCustomSelect.vue';
import AppLabel from './components/AppLabel.vue';

export default {
  name: 'App',

  data() {
    return {
      info: [
        {
          name: 'Имя',
          value: '',
          type: 'text',
          placeholder: 'Введите ваше имя',
          pattern: /^[^0-9]{2,30}$/
        },
        {
          name: 'Email',
          value: '',
          type: 'text',
          placeholder: 'Введите ваше email',
          pattern: /^.+@.+\..+$/
        },
        {
          name: 'Номер телефона',
          value: '',
          type: 'tel',
          placeholder: 'Введите номер телефона',
          pattern: /^((8|\+7)[\- ]?)?(\(?\d{3}\)?[\- ]?)?[\d\- ]{7,10}$/
        }
      ],

      language: '',
      list: ['Русский', 'Английский', 'Немецкий'],
      options: [
        { name: 'Русский', value: 1 },
        { name: 'Английский', value: 2 },
        { name: 'Китайский', value: 3 },
        { name: 'Испанский', value: 4 }
      ],

      confirm: '',
      formDone: false
    };
  },
  methods: {
    optionSelected(option) {
      this.language = option.name;
    },
    onInput(i, value) {
      let field = this.info[i];
      field.value = value.trim();
      field.valid = field.pattern.test(field.value);
    },
    sendForm() {
      if (this.formReady) {
        this.formDone = true;
      }
    },
    backToForm() {
      this.info.forEach(label => (label.value = ''));
      this.confirm = this.language = '';
      this.formDone = !this.formDone;
    }
  },
  computed: {
    fieldsDone() {
      return this.info.reduce(
        (total, field) => total + (field.valid ? 1 : 0),
        0
      );
    },
    fieldsReady() {
      return this.fieldsDone === this.info.length;
    },
    formReady() {
      return this.fieldsReady && this.language !== '' && this.confirm;
    }
  },

  components: { AppCustomSelect, AppLabel },

  created() {
    return this.info.forEach(field => {
      field.valid = field.pattern.test(field.value);
    });
  }
};
</script>
