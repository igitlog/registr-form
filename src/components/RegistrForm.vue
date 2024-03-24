<template>
  <div>
    <div v-if="!success" class="content-wrapper">
      <h1>{{ msg }}</h1>
      <div class="registrForm">
        <div class="registrForm__title">Регистрация</div>
        <el-form ref="form" :model="formData" :rules="rules" label-width="120px">
        <div class="registrForm__body">
          <div class="registrForm__body-title">Заполните Ваши данные</div>
          
          <el-row class="registrForm__body-row">
            <el-col :span="12"><InputEl prop="username" @update:modelValue="handleUpdate" :rules="rules.username" v-model="formData.username" placeholder="Введите имя пользователя"></InputEl></el-col>
            <el-col :span="12"><InputEl prop="email" @update:modelValue="handleUpdate" :rules="rules.email" v-model="formData.email" placeholder="Введите email" type="email"></InputEl></el-col>
          </el-row>
          <el-row class="registrForm__body-row">
            <el-col :span="12" :offset="12">
              <el-form-item prop="role" style="margin-left:8px;">
                <el-select v-model="formData.role" placeholder="Выберите должность">
                  <el-option v-for="option in positionOptions" :key="option.value" :value="option.value" :label="option.name"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <el-row class="registrForm__body-row">
            <el-col :span="12"><InputEl prop="password" :showPassword="true" @update:modelValue="handleUpdate" :rules="rules.password" v-model="formData.password" placeholder="Введите пароль" type="password"></InputEl></el-col>
            <el-col :span="12"><InputEl prop="password_repeat" :showPassword="true" @update:modelValue="handleUpdate" :rules="rules.password_repeat" v-model="formData.password_repeat" placeholder="Повторите пароль" type="password"></InputEl></el-col>
          </el-row>
        </div>
        <div class="registrForm__footer">
          <el-row>
            <el-form-item class="registrForm__footer-subscribe" label-width="auto" label="Хотите чтобы Ваш профиль видели другие участники платформы?">
              <el-switch style="margin-right: 5px;" v-model="formData.switchValue" active-color="#3586FF" inactive-color="#b6b6b6"></el-switch>
            </el-form-item>
          </el-row>
          <span class="registrForm__footer-subscribe-text">Включает профиль для просмотра другими пользователями по ссылке</span>
          <el-row>
            
          </el-row>
          <el-row class="registrForm__footer-row-agree">
            <el-form-item class="registrForm__footer-agree" prop="agree" :rules="rules.agree" >
              <el-checkbox v-model="formData.agree" label="agree">
                <span>
                  Регистрируясь, Вы соглашаетесь с 
                  <el-link type="primary" @click.prevent="showPrivacyPolicy">политикой конфиденциальности</el-link> 
                  и обработкой
                  <el-link type="primary" @click.prevent="showDataProcessingPolicy"> персональных данных</el-link>
                </span>
              </el-checkbox>
            </el-form-item>
            <el-button class="custom-btn" @click="submitForm" style="width: 302px;">Зарегистрироваться</el-button>
          </el-row>
        </div>
        </el-form>
      </div>
    </div>
    <div v-if="success" style="margin-top: 20px; color: green;">Регистрация успешно завершена</div>
  </div>
</template>

<script>
import InputEl from './uikit/InputEl.vue'
export default {
  name: 'RegistrForm',
  components: {
    InputEl,
  },
  props: {
    msg: String
  },
  data() {
    return {
      formData: {
        username: '',
        role: '',
        email: '',
        password: '',
        password_repeat: '',
        switchValue: true,
        agree: true // по умолчанию согласие на обработку данных установлено
      },
      positionOptions: [ // варианты для селекта "Должность"
        { value: 1, name: 'Менеджер' },
        { value: 2, name: 'Разработчик' },
        { value: 3, name: 'Дизайнер' }
      ],
      rules: {
        username: [{ required: true, message: 'Пожалуйста, введите имя пользователя', trigger: 'blur' }],
        email: [
          { required: true, message: 'Пожалуйста, введите email', trigger: 'blur' },
          { type: 'email', message: 'Пожалуйста, введите корректный email', trigger: ['blur', 'change'] }
        ],
        role: [
          { required: true, message: 'Пожалуйста, выберите роль', trigger: ['blur', 'change'] },
        ],
        password: [{ required: true, message: 'Пожалуйста, введите пароль', trigger: 'blur' }],
        password_repeat: [
          { required: true, message: 'Пожалуйста, введите пароль еще раз', trigger: 'blur' },
          { validator: this.validatePasswordRepeat, trigger: 'blur' }
        ],
        switchValue: [{ required: false}],
        agree: [{ required: true, message: 'Пожалуйста, согласитесь на обработку персональных данных', trigger: 'change', validator: (rule, value, callback) => {
        if (!value) {
          callback(new Error('Пожалуйста, согласитесь на обработку персональных данных'));
        } else {
          callback();
        }
      }}]
      },
      success: false
    };
  },
  methods: {
    submitForm() {
      this.$refs.form.validate(valid => {
        if (valid) {
          // отправка данных
          const postData = {
            public: this.formData.agree,
            username: this.formData.username,
            role: this.formData.role,
            email: this.formData.email,
            agree: this.formData.agree,
            password: this.formData.password,
            password_repeat: this.formData.password_repeat
          };
          console.log(postData); // здесь вы можете отправить данные на сервер
          // сделаем вид, что отправка прошла успешно
          this.success = true;
        } else {
          return false;
        }
      });
    },
    validatePasswordRepeat(rule, value, callback) {
      if (value !== this.formData.password) {
        callback(new Error('Пароли не совпадают'));
      } else {
        callback();
      }
    },
    handleUpdate(value, propName) {
      console.log('Received value:', value);
      console.log('For prop:', propName);
      // здесь вы можете обновить formData.username
      if (propName === 'username') {
        this.formData.username = value;
      } else if (propName === 'email') {
        this.formData.email = value;
      } else if (propName === 'password_repeat') {
        this.formData.password_repeat = value;
      } else if (propName === 'password') {
        this.formData.password = value;
      }
    },
  }
}
</script>

<style lang="scss">
html {
  height: 100%;
  width: 100%;
  background: #e6e6e6;
}
.content-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.el-select {
  width: 100%;
}
.registrForm {
  max-width: 960px;
  background: #FFFFFF;
  border-radius: 15px;
  font-family: Montserrat;
  &__title {
    font-size: 19px;
    font-weight: 700;
    line-height: 27px;
    letter-spacing: -0.0035em;
    text-align: left;
    padding: 17px 31px;
    border-bottom: 1px solid #D9D9D9;
  }
  &__body {
    padding: 17px 31px 30px 31px;
    border-bottom: 1px solid #D9D9D9;
  }
  &__body-title {
    font-size: 16px;
    font-weight: 500;
    line-height: 19px;
    text-align: left;
    padding-bottom: 34px;
  }
  &__body-row {
    display: flex;
    gap: 14px;
    justify-content: center;
    &:before {
      content: none;
    }
    &:after {
      content: none;
    }
  }
  &__footer {

  }
  &__footer-subscribe {
    display: flex;
    align-items: center;
    flex-direction: row-reverse;
    justify-content: flex-end;
    padding: 23px 31px 30px 31px;
    padding-bottom: 0;
    margin-bottom: 0;
  }
  &__footer-subscribe-text {
    width: 100%;
    display: inline-block;
    text-align: left;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    color: #696977;
    padding: 0 31px 30px 31px;
  }
  &__footer-agree {
    margin: 0;
  }
  &__footer-row-agree {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    padding: 0 15px 30px 31px;
    gap: 10px;
    &:before {
      content: none;
    }
    &:after {
      content: none;
    }
  }
}
.el-form-item__content {
  margin: 0!important;
  max-width: 541px;
  & .el-checkbox {
    display: flex;
    text-wrap: wrap;
  }
}
.el-select input {
  border: 1px solid #E6E6EB;
  font-family: Montserrat;
  font-size: 14px;
  font-weight: 400;
  line-height: 19px;
  letter-spacing: -0.0015em;
  text-align: left;
  border-radius: 11px;
}
.el-checkbox__label {
  font-size: 14px;
  font-weight: 400;
  line-height: 19px;
  letter-spacing: -0.0015em;
  text-align: left;
  color: #000000
}
.el-checkbox__input.is-checked+.el-checkbox__label {
  color: #000000
}
.custom-btn {
  background: #497ADA33;
}
</style>
