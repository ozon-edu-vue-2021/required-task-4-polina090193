<template>
  <div class="citizenship-wrapper">
    <label class="label label_choose" for="citizenship">
      <span>Гражданство</span>
      <input
        v-model="citizenship"
        id="citizenship"
        @focus="isDropdownOpen = true"
        class="input input_choose"
      />
    </label>

    <fieldset class="fieldset fieldset-rus" v-if="isRus">
      <label for="serie" class="label label_for-text">
        <span>Серия паспорта</span>
        <input
          type="text"
          id="serie"
          class="input input_passport input_serie"
        />
      </label>

      <label for="pass-num" class="label label_for-text">
        <span>Номер паспорта</span>
        <input
          type="text"
          id="pass-num"
          class="input input_passport input_pass-num"
        />
      </label>

      <label for="pass-date" class="label label_for-date">
        <span>Дата выдачи</span>
        <input type="date" id="pass-date" class="input input_date" />
      </label>
    </fieldset>

    <fieldset class="fieldset fieldset-abroad" v-if="isAbroad">
      <label for="lastname-lat" class="label label_for-text">
        <span>Фамилия на латинице</span>
        <input type="text" id="lastname-lat" class="input input_text" />
      </label>

      <label for="firstname-lat" class="label label_for-text">
        <span>Имя на латинице</span>
        <input type="text" id="firstname-lat" class="input input_text" />
      </label>

      <label for="pass-num-abroad" class="label label_for-text">
        <span>Номер паспорта</span>
        <input
          type="text"
          id="pass-num-abroad"
          class="input input_passport-abroad input_pass-num-abroad"
        />
      </label>
      <label for="pass-date" class="label label_for-date">
        <span>Дата выдачи</span>
        <input type="date" id="pass-date" class="input input_date" />
      </label>
      <label for="citizenship-abroad" class="label label_choose">
        <span>Страна выдачи</span>
        <input
          v-model="chooseCitizenship"
          id="citizenship-abroad"
          @focus="isDropdownOpen = true"
          class="input input_choose"
        />
      </label>
      <label for="passtype-abroad" class="label label_choose">
        <span>Тип паспорта</span>
        <input
          v-model="choosePasstype"
          id="passtype-abroad"
          @focus="isDropdownOpen = true"
          class="input input_choose"
        />
      </label>
    </fieldset>

    <fieldset class="fieldset fieldset-radio">
      <legend class="fieldset-legend">Меняли ли фамилию или имя?</legend>
      <label for="namechange-no" class="label label_for-radio">
        <span>Нет</span>
        <input
          type="radio"
          id="namechange-no"
          name="namechange"
          value="no"
          class="input input_radio"
          v-model="nameChangeVal"
        />
      </label>
      <label for="namechange-yes" class="label label_for-radio">
        <span>Да</span>
        <input
          type="radio"
          id="namechange-yes"
          name="namechange"
          value="yes"
          class="input input_radio"
          v-model="nameChangeVal"
        />
      </label>
    </fieldset>

    <fieldset class="fieldset-namechenged" v-if="nameChanged">
      <label for="lastname-changed" class="label label_for-text">
        <span>Фамилия предыдущая</span>
        <input type="text" id="lastname-changed" class="input input_text" />
      </label>
      <label for="firstname-changed" class="label label_for-text">
        <span>Имя предыдущее</span>
        <input type="text" id="firstname-changed" class="input input_text" />
      </label>
    </fieldset>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isRus: false,
      isAbroad: false,
      nameChangeVal: null,
      nameChanged: false,
      citizenship: null,
    };
  },
  watch: {
    nameChangeVal(value) {
      this.nameChanged = value === "yes";
    },
  },
  methods: {},
};
</script>

<style scoped>
.citizenship-wrapper {
  margin-bottom: 14px;
}
</style>
