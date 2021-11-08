<template>
  <form class="form" @submit.prevent="validateForm">
    <label for="lastname" class="label label_for-text">
      <span>Фамилия*</span>
      <input
        type="text"
        id="lastname"
        :class="[
          'input',
          'input_text',
          { warning: this.errors.includes('lastName') },
        ]"
        @input="clearWarning('lastName')"
        v-model="formData.lastName.value"
      />
    </label>
    <label for="firstname" class="label label_for-text">
      <span>Имя*</span>
      <input
        type="text"
        id="firstname"
        :class="[
          'input',
          'input_text',
          { warning: this.errors.includes('firstName') },
        ]"
        @input="clearWarning('firstName')"
        v-model="formData.firstName.value"
      />
    </label>
    <label for="fathername" class="label label_for-text">
      <span>Отчество</span>
      <input
        type="text"
        id="fathername"
        :class="[
          'input',
          'input_text',
          { warning: this.errors.includes('fatherName') },
        ]"
        @input="clearWarning('fatherName')"
        v-model="formData.fatherName.value"
      />
    </label>

    <label for="birth" class="label label_for-date">
      <span>Дата рождения*</span>
      <input
        type="date"
        id="birth"
        :class="[
          'input',
          'input_date',
          { warning: this.errors.includes('birthDate') },
        ]"
        @input="clearWarning('birthDate')"
        v-model="formData.birthDate.value"
      />
    </label>

    <label for="email" class="label label_for-email">
      <span>E-mail*</span>
      <input
        type="email"
        id="email"
        :class="[
          'input',
          'input_email',
          { warning: this.errors.includes('email') },
        ]"
        @input="clearWarning('email')"
        v-model="formData.email.value"
      />
    </label>

    <fieldset class="fieldset fieldset-radio">
      <legend class="fieldset-legend">Пол</legend>
      <label for="gender" class="label label_for-radio">
        <span>Мужской</span>
        <input
          type="radio"
          id="gender"
          name="gender"
          class="input input_radio"
          value="Мужской"
          v-model="formData.gender.value"
          checked
        />
      </label>
      <label for="gender" class="label label_for-radio">
        <span>Женский</span>
        <input
          type="radio"
          id="gender"
          name="gender"
          class="input input_radio"
          value="Женский"
          v-model="formData.gender.value"
        />
      </label>
    </fieldset>

    <Citizenship
      :selector-type="'citizen'"
      @citizenship-set="onCitizenShipSet"
      @no-citizenship="noCitizenShipSelected"
    />

    <fieldset class="fieldset fieldset-rus" v-if="isRus">
      <label for="serie" class="label label_for-text">
        <span>Серия паспорта*</span>
        <input
          type="text"
          id="serie"
          :class="[
            'input',
            'input_passport',
            'input_serie',
            { warning: this.errors.includes('serie') },
          ]"
          @input="clearWarning('serie')"
          v-model="formData.serie.value"
        />
      </label>

      <label for="pass-num" class="label label_for-text">
        <span>Номер паспорта*</span>
        <input
          type="text"
          id="pass-num"
          :class="[
            'input',
            'input_passport',
            'input_pass-num',
            { warning: this.errors.includes('rusNum') },
          ]"
          @input="clearWarning('rusNum')"
          v-model="formData.rusNum.value"
        />
      </label>

      <label for="pass-date" class="label label_for-date">
        <span>Дата выдачи*</span>
        <input
          type="date"
          id="pass-date"
          :class="[
            'input',
            'input_date',
            { warning: this.errors.includes('rusDate') },
          ]"
          @input="clearWarning('rusDate')"
          v-model="formData.rusDate.value"
        />
      </label>
    </fieldset>

    <fieldset class="fieldset fieldset-abroad" v-if="isAbroad">
      <label for="lastname-lat" class="label label_for-text">
        <span>Фамилия на латинице*</span>
        <input
          type="text"
          id="lastname-lat"
          :class="[
            'input',
            'input_text',
            { warning: this.errors.includes('lastNameLat') },
          ]"
          @input="clearWarning('lastNameLat')"
          v-model="formData.lastNameLat.value"
        />
      </label>

      <label for="firstname-lat" class="label label_for-text">
        <span>Имя на латинице*</span>
        <input
          type="text"
          id="firstname-lat"
          :class="[
            'input',
            'input_text',
            { warning: this.errors.includes('firstNameLat') },
          ]"
          @input="clearWarning('firstNameLat')"
          v-model="formData.firstNameLat.value"
        />
      </label>

      <label for="pass-num-abroad" class="label label_for-text">
        <span>Номер паспорта*</span>
        <input
          type="text"
          id="pass-num-abroad"
          :class="[
            'input',
            'input_passport-abroad',
            'input_pass-num-abroad',
            { warning: this.errors.includes('abroadNum') },
          ]"
          @input="clearWarning('abroadNum')"
          v-model="formData.abroadNum.value"
        />
      </label>
      <Citizenship
        :selector-type="'issue-country'"
        @country-set="onCountrySet"
        @no-country="noCountrySelected"
      />
      <PassType @passtype-set="onPassTypeSet" />
    </fieldset>

    <fieldset class="fieldset fieldset-radio">
      <legend class="fieldset-legend">Меняли ли фамилию или имя?</legend>
      <label for="namechange-no" class="label label_for-radio">
        <span>Нет</span>
        <input
          type="radio"
          id="namechange-no"
          name="namechange"
          value="Нет"
          class="input input_radio"
          v-model="nameChangeVal"
          checked
        />
      </label>
      <label for="namechange-yes" class="label label_for-radio">
        <span>Да</span>
        <input
          type="radio"
          id="namechange-yes"
          name="namechange"
          value="Да"
          class="input input_radio"
          v-model="nameChangeVal"
        />
      </label>
    </fieldset>

    <fieldset class="fieldset-namechenged" v-if="nameChanged">
      <label for="lastname-changed" class="label label_for-text">
        <span>Фамилия предыдущая</span>
        <input
          type="text"
          id="lastname-changed"
          :class="[
            'input',
            'input_text',
            { warning: this.errors.includes('prevLastName') },
          ]"
          @input="clearWarning('prevLastName')"
          v-model="formData.prevLastName.value"
        />
      </label>
      <label for="firstname-changed" class="label label_for-text">
        <span>Имя предыдущее</span>
        <input
          type="text"
          id="firstname-changed"
          :class="[
            'input',
            'input_text',
            { warning: this.errors.includes('prevFirstName') },
          ]"
          @input="clearWarning('prevFirstName')"
          v-model="formData.prevFirstName.value"
        />
      </label>
    </fieldset>

    <input type="submit" class="submit" value="Отправить" />
  </form>
</template>

<script>
import Citizenship from "./Citizenship.vue";
import PassType from "./PassType.vue";

export default {
  components: {
    Citizenship,
    PassType,
  },

  data() {
    return {
      validateInput: {
        rusOnly: (value) => /^[А-ЯЁ]+$/i.test(value),
        enOnly: (value) => /^[A-Z]+$/i.test(value),
        date: (...value) => {
          const dateValue = new Date(...value),
            dateCurrent = new Date();
          return (
            !Number.isNaN(dateValue.valueOf()) &&
            dateValue.getTime() < dateCurrent.getTime()
          );
        },
        email: (value) => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value),
        serie: (value) => /^\d{4}$/.test(value),
        numRus: (value) => /^\d{6}$/.test(value),
        numAbroad: (value) => /^\d$/.test(value),
      },
      isRus: false,
      isAbroad: false,
      nameChangeVal: "Нет",
      nameChanged: false,
      citizenship: {
        value: null,
        required: true,
      },
      errors: [],
    };
  },
  computed: {
    formData() {
      return {
        lastName: {
          value: null,
          required: true,
          validator: "rusOnly",
        },
        firstName: {
          value: null,
          required: true,
          validator: "rusOnly",
        },
        fatherName: {
          value: null,
          required: false,
          validator: "rusOnly",
        },
        birthDate: {
          value: null,
          required: true,
          validator: "date",
        },
        email: {
          value: null,
          required: true,
          validator: "email",
        },
        gender: {
          value: "Мужской",
        },
        citizenship: this.citizenship,
        serie: {
          value: null,
          required: this.isRus,
          validator: "serie",
        },
        rusNum: {
          value: null,
          required: this.isRus,
          validator: "numRus",
        },
        rusDate: {
          value: null,
          required: this.isRus,
          validator: "date",
        },
        lastNameLat: {
          value: null,
          required: this.isAbroad,
          validator: "enOnly",
        },
        firstNameLat: {
          value: null,
          required: this.isAbroad,
          validator: "enOnly",
        },
        abroadNum: {
          value: null,
          required: this.isAbroad,
          validator: "numAbroad",
        },
        passType: {
          value: null,
          required: this.isAbroad,
        },
        country: {
          value: null,
          required: this.isAbroad,
        },
        prevLastName: {
          value: null,
          required: false,
          validator: "rusOnly",
        },
        prevFirstName: {
          value: null,
          required: false,
          validator: "rusOnly",
        },
      };
    },
  },
  provide() {
    return {
      removeWarning: this.removeWarning,
      validateInput: this.validateInput,
      formData: this.formData,
    };
  },
  watch: {
    nameChangeVal(newValue) {
      this.nameChanged = newValue === "Да";
    },
  },
  methods: {
    onCitizenShipSet(citizenShip) {
      this.formData.citizenship.value = citizenShip;
      if (citizenShip === "Russia") {
        this.isRus = true;
        this.isAbroad = false;
      } else {
        this.isRus = false;
        this.isAbroad = true;
      }
    },
    noCitizenShipSelected() {
      this.isRus = false;
      this.isAbroad = false;
      this.formData.citizenship.value = null;
    },
    onCountrySet(countrySelected) {
      this.formData.country.value = countrySelected;
    },
    noCountrySelected() {
      this.formData.country.value = null;
    },
    onPassTypeSet(type) {
      this.formData.passType.value = type;
    },
    clearWarning(name) {
      this.errors.splice(this.errors.indexOf(name), 1);
    },
    validateForm() {
      const data = this.formData;

      for (let key in data) {
        let item = data[key];
        if (item.required && !item.value) {
          this.errors.push(key);
        } else if (
          item.value &&
          item.validator &&
          !this.validateInput[item.validator](item.value)
        ) {
          this.errors.push(key);
        }
      }

      if (!this.errors.length) this.submitForm();
    },

    submitForm() {
      alert("Форма отправлена!");
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
  outline: red solid 2px;
}
.input.warning::after {
  content: "Проверьте правильность заполнения поля";
  display: block;
  font-size: 10px;
  color: red;
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
.selector {
  position: relative;
}
.selector___dropdown {
  position: absolute;
  top: calc(100% - 14px);
  max-height: 200px;
  overflow-y: auto;
  width: 100%;
  background: white;
  border-radius: 10px;
  color: rgb(37, 37, 41);
}
.selector___dropdown ul {
  padding-left: 0;
}
.selector___dropdown li,
.empty {
  padding: 4px 10px;
  list-style: none;
  cursor: pointer;
}
.selector___dropdown li:hover {
  background-color: rgb(37, 37, 41);
  color: lightgrey;
}
</style>
