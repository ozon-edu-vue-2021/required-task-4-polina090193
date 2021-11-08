<template>
  <form class="form" @submit="validateForm">
    <label for="lastname" class="label label_for-text">
      <span>Фамилия*</span>
      <input
        type="text"
        id="lastname"
        class="input input_text"
        v-model="formData.lastName.value"
      />
    </label>
    <label for="firstname" class="label label_for-text">
      <span>Имя*</span>
      <input type="text" id="firstname" class="input input_text" v-model="formData.firstName.value" />
    </label>
    <label for="fathername" class="label label_for-text">
      <span>Отчество</span>
      <input type="text" id="fathername" class="input input_text"  v-model="formData.fatherName.value"/>
    </label>

    <label for="birth" class="label label_for-date">
      <span>Дата рождения*</span>
      <input type="date" id="birth" class="input input_date" v-model="formData.birthDate.value" />
    </label>

    <label for="email" class="label label_for-email">
      <span>E-mail*</span>
      <input type="email" id="email" class="input input_email" v-model="formData.email.value" />
    </label>

    <fieldset class="fieldset fieldset-radio">
      <legend class="fieldset-legend">Пол*</legend>
      <label for="gender" class="label label_for-radio">
        <span>Мужской</span>
        <input type="radio" id="gender" name="gender" class="input input_radio" v-model="formData.gender.value" />
      </label>
      <label for="gender" class="label label_for-radio">
        <span>Женский</span>
        <input type="radio" id="gender" name="gender" class="input input_radio" v-model="formData.gender.value" />
      </label>
    </fieldset>

    <Passport @passdata-updated="updatePassData" />

    <input type="submit" class="submit" value="Отправить" />
  </form>
</template>

<script>
import Passport from "./Passport.vue";

export default {
  components: {
    Passport,
  },

  data() {
    return {
      notValidInputs: new Map(),
      passData: {},
    };
  },
  computed: {
    formIsValid() {
      return !this.notValidInputs.size;
    },
    formData() {
      return {
        lastName: {
          value: null,
          required: true,
        },
        firstName: {
          value: null,
          required: true,
        },
        fatherName: {
          value: null,
          required: false,
        },
        birthDate: {
          value: null,
          required: true,
        },
        email: {
          value: null,
          required: true,
        },
        gender: {
          value: null,
          required: true,
        },
      };
    },
  },
  methods: {
    inputIsNotValid(input) {
      input.classList.add("warning");
      this.notValidInputs.add(input);
    },
    inputIsValid(input) {
      input.classList.remove("warning");
      this.notValidInputs.remove(input);
    },
    validateForm(e) {
      e.preventDefault();
      const data = { ...this.formData, ...this.passData };
      console.log(data);
      // this.submitForm();
    },
    submitForm() {
      alert("Форма отправлена!");
    },
    updatePassData(data) {
      this.passData = data;
    },
  },
};
</script>

<style>
.form {
  display: flex;
  flex-direction: column;
  width: 300px;
  margin: auto;
}
.input {
  border: black solid 1px;
  border-radius: 10px;
  padding: 4px 10px;
}
.input:focus {
  outline: white solid 1px;
}
.input,
.fieldset {
  width: 100%;
  margin-bottom: 14px;
}
.input.warning {
  outline: red solid 1px;
}
.label {
  margin-bottom: 4px;
  font-size: 14px;
  line-height: 24px;
}
.label:not(.label_for-radio) {
  flex-direction: column;
}
.label_for-radio {
  display: flex;
  margin-right: 20px;
}
.input_radio {
  width: auto;
}
.input_radio:focus {
  outline: none;
}
.submit {
  width: 140px;
  height: 40px;
  margin-left: auto;
  font-size: 16px;
  border-radius: 10px;
  cursor: pointer;
}
</style>
