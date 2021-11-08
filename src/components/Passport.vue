<template>
  <div class="citizenship-wrapper">
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
          class="input input_passport input_serie"
          v-model="passData.serie.value"
        />
      </label>

      <label for="pass-num" class="label label_for-text">
        <span>Номер паспорта*</span>
        <input
          type="text"
          id="pass-num"
          class="input input_passport input_pass-num"
          v-model="passData.rusNum.value"
        />
      </label>

      <label for="pass-date" class="label label_for-date">
        <span>Дата выдачи*</span>
        <input
          type="date"
          id="pass-date"
          class="input input_date"
          v-model="passData.rusDate.value"
        />
      </label>
    </fieldset>

    <fieldset class="fieldset fieldset-abroad" v-if="isAbroad">
      <label for="lastname-lat" class="label label_for-text">
        <span>Фамилия на латинице*</span>
        <input
          type="text"
          id="lastname-lat"
          class="input input_text"
          v-model="passData.lastNameLat.value"
        />
      </label>

      <label for="firstname-lat" class="label label_for-text">
        <span>Имя на латинице*</span>
        <input
          type="text"
          id="firstname-lat"
          class="input input_text"
          v-model="passData.firstNameLat.value"
        />
      </label>

      <label for="pass-num-abroad" class="label label_for-text">
        <span>Номер паспорта*</span>
        <input
          type="text"
          id="pass-num-abroad"
          class="input input_passport-abroad input_pass-num-abroad"
          v-model="passData.abroadNum.value"
        />
      </label>
      <Citizenship
        :selector-type="'issue-country'"
        @country-set="onCountrySet"
        @no-country="noCountrySelected"
      />
      <PassType />
    </fieldset>

    <fieldset class="fieldset fieldset-radio">
      <legend class="fieldset-legend">Меняли ли фамилию или имя?*</legend>
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
        <input
          type="text"
          id="lastname-changed"
          class="input input_text"
          v-model="passData.prevLastName.value"
        />
      </label>
      <label for="firstname-changed" class="label label_for-text">
        <span>Имя предыдущее</span>
        <input
          type="text"
          id="firstname-changed"
          class="input input_text"
          v-model="passData.prevFirstName.value"
        />
      </label>
    </fieldset>
  </div>
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
      isRus: false,
      isAbroad: false,
      nameChangeVal: null,
      nameChanged: false,
      citizenship: {
        value: null,
        required: true,
      },
    };
  },
  watch: {
    nameChangeVal(newValue) {
      this.nameChanged = newValue === "yes";
    },
    passData(newValue) {
      this.$emit("passdata-updated", newValue);
    },
  },
  computed: {
    passData() {
      return {
        citizenship: this.citizenship,
        serie: {
          value: null,
          required: this.isRus,
        },
        rusNum: {
          value: null,
          required: this.isRus,
        },
        rusDate: {
          value: null,
          required: this.isRus,
        },
        lastNameLat: {
          value: null,
          required: this.isAbroad,
        },
        firstNameLat: {
          value: null,
          required: this.isAbroad,
        },
        abroadNum: {
          value: null,
          required: this.isAbroad,
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
        },
        prevFirstName: {
          value: null,
          required: false,
        },
      };
    },
  },
  methods: {
    onCitizenShipSet(citizenShip) {
      this.passData.citizenship.value = citizenShip;
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
      this.passData.citizenship.value = null;
    },
    onCountrySet(countrySelected) {
      this.passData.country.value = countrySelected;
    },
    noCountrySelected() {
      this.passData.country.value = null;
    },
  },
};
</script>

<style scoped>
.citizenship-wrapper {
  margin-bottom: 14px;
}
</style>
