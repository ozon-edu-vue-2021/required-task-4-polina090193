<template>
  <form class="form">
    <label for="lastname" class="label label_for-text">
      <span>Фамилия</span>
      <input type="text" id="lastname" class="input input_text" />
    </label>
    <label for="firstname" class="label label_for-text">
      <span>Имя</span>
      <input type="text" id="firstname" class="input input_text" />
    </label>
    <label for="fathername" class="label label_for-text">
      <span>Отчество</span>
      <input type="text" id="fathername" class="input input_text" />
    </label>

    <label for="birth" class="label label_for-date">
      <span>Дата рождения</span>
      <input type="date" id="birth" class="input input_date" />
    </label>

    <label for="email" class="label label_for-email">
      <span>E-mail</span>
      <input type="email" id="email" class="input input_email" />
    </label>

    <fieldset class="fieldset fieldset-radio">
      <legend class="fieldset-legend">Пол</legend>
      <label for="sex" class="label label_for-radio">
        <span>Мужской</span>
        <input type="radio" id="sex" name="sex" class="input input_radio" />
      </label>
      <label for="sex" class="label label_for-radio">
        <span>Женский</span>
        <input type="radio" id="sex" name="sex" class="input input_radio" />
      </label>
    </fieldset>

    <Passport />

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
    return {};
  },
  methods: {},
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
.label,
.fieldset {
  margin-bottom: 14px;
  font-size: 14px;
  line-height: 24px;
}
.label span {
  margin-bottom: 4px;
}
.label:not(.label_for-radio) {
  display: flex;
  flex-direction: column;
}
.submit {
  width: 140px;
  height: 40px;
  margin-left: auto;
  font-size: 16px;
  border-radius: 10px;
}
</style>
