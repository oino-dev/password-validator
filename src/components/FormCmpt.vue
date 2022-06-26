<template>
  <div class="form-cmpt">
    <div class="content">
      <h1>Change password</h1>
      <div class="field-block">
        <custom-input
          v-model="password"
          :type="typePassword"
          label="Password"
          placeholder="Input your password"
          @input="checkPassword"
        />
        <span :class="weak.class" class="weakness-msg">
          {{ weak.msg }}
        </span>
      </div>
      <validator-cmpt :validators="validators" />
      <div class="field-block">
        <custom-input
          v-model="repeatPassword"
          :type="typePassword"
          label="Repeat password"
          placeholder="Repeat your password"
          @input="checkPassword"
        />
        <div class="msg-block">
          <p>{{ showMsg }}</p>
          <span
            class="show-password"
            @click="showPassword"
            @mouseleave="hidePassword"
          >
            Показать пароль
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import CustomInput from "@/components/CustomInput.vue";
import ValidatorCmpt from "@/components/ValidatorCmpt.vue";

export interface IValidator {
  hasMatch: boolean;
  hasSpecChar: boolean;
  hasNumber: boolean;
  hasLatinLowercase: boolean;
  hasLatinUppercase: boolean;
  hasCyrillic: boolean;
  hasLength: boolean;
  hasSpace: boolean;
}

@Component({
  components: { ValidatorCmpt, CustomInput },
})
export default class FormCmpt extends Vue {
  typePassword = "password";
  password = "";
  repeatPassword = "";
  isValid = false;
  showMsg = "";
  weak: { msg: string; class: string } = { msg: "", class: "" };
  validators: IValidator = {
    hasMatch: false,
    hasLength: false,
    hasNumber: false,
    hasSpecChar: false,
    hasLatinLowercase: false,
    hasLatinUppercase: false,
    hasCyrillic: false,
    hasSpace: false,
  };

  hidePassword(): void {
    this.typePassword = "password";
  }

  showPassword(): void {
    this.typePassword = "text";
  }

  checkPassword(): void {
    const validate = this.validators;
    const { password, repeatPassword } = this;
    const filter = {
      isNumber: new RegExp(/\d/),
      isLatinLowercase: new RegExp(/[a-z]/),
      isLatinUppercase: new RegExp(/[A-Z]/),
      isCyrillic: new RegExp(/[а-яА-ЯёЁ]/),
      isSpecialChar: new RegExp(/[?!@#$%^&*()]/),
      isSpace: new RegExp(/\s/),
    };

    this.isValid = false;

    validate.hasLength = password.length >= 8;
    validate.hasMatch = password === repeatPassword;
    validate.hasNumber = filter.isNumber.test(password);
    validate.hasCyrillic = filter.isCyrillic.test(password);
    validate.hasLatinLowercase = filter.isLatinLowercase.test(password);
    validate.hasLatinUppercase = filter.isLatinUppercase.test(password);
    validate.hasSpecChar = filter.isSpecialChar.test(password);
    validate.hasSpace = filter.isSpace.test(password);

    this.currentMsg();
    this.weakness();
  }

  weakness(): void {
    if (this.password.match(/((?=.*[A-Z]).{12,15})/)) {
      this.weak = { msg: "Сильный пароль", class: "strong" };
    } else if (this.password.match(/((?=.*[a-z]).{10,11})/)) {
      this.weak = { msg: "Средний пароль", class: "middle" };
    } else if (
      this.password.match(/(([a-z]).{8,9})/) ||
      this.password.length <= 7
    ) {
      this.weak = { msg: "Слабый пароль", class: "weak" };
    }
  }

  currentMsg(): void {
    if (this.password || this.repeatPassword) {
      if (!this.validators.hasLength) {
        this.showMsg = "Длина пароля должна быть больше 8 символов";
      } else if (this.validators.hasSpace) {
        this.showMsg = "Пароль не может содержать пробелы";
      } else if (this.validators.hasCyrillic) {
        this.showMsg = "Пароль не может содержать русские буквы";
      } else if (!this.validators.hasLatinLowercase) {
        this.showMsg =
          "Пароль должен содержать хотя бы одну букву в нижнем регистре";
      } else if (!this.validators.hasLatinUppercase) {
        this.showMsg =
          "Пароль должен содержать хотя бы одну букву в верхнем регистре";
      } else if (!this.validators.hasNumber) {
        this.showMsg = "Пароль должен содержать хотя бы одну цифру";
      } else if (!this.validators.hasSpecChar) {
        this.showMsg = "Пароль должен содержать хотя бы один спецсимвол";
      } else if (!this.validators.hasMatch) {
        this.showMsg = "Пароли не совпадают";
      } else {
        this.showMsg = "";
        this.isValid = true;
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/styles/index";

.form-cmpt {
  align-items: center;
  display: flex;
  height: 100vh;
  justify-content: center;
  width: 100vw;
}

.content {
  background: $bg-color;
  border-radius: 3em;
  display: flex;
  flex-flow: column wrap;
  height: 500px;
  padding: 20px;
  width: 500px;
}

h1 {
  color: $base-text-color;
  padding: 0;
  text-align: center;
}

.msg-block {
  & > p {
    color: red;
    font-size: 12px;
    height: 24px;
    margin: 5px 0 0 15px;
    min-width: 250px;
    padding: 0;
  }
}

.show-password {
  color: #419aff;
  cursor: pointer;
  display: inline-block;
  font-size: 11px;
  height: 20px;
  margin-top: 7px;
  text-align: right;
  width: 100px;

  &:hover {
    text-decoration: underline;
  }
}
</style>
