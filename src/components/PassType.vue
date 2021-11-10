<template>
  <div class="selector" v-click-outside="hideDropdown">
    <label class="label label_choose" for="pass-type">Тип паспорта*</label>
    <input
      v-model="passType"
      id="pass-type"
      @focus="isDropdownOpen = true"
      class="input input_choose"
    />
    <div v-if="isDropdownOpen" class="selector___dropdown">
      <ul v-if="allPassTypes.length">
        <li
          v-for="passtype in allPassTypes"
          :key="passtype.id"
          @click="onPassTypeClick(passtype.type)"
        >
          {{ passtype.type }}
        </li>
      </ul>
      <div v-else class="empty">Ничего не найдено</div>
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import passTypes from "../assets/data/passport-types.json";

export default {
  data() {
    return {
      isDropdownOpen: false,
      passType: null,
      allPassTypes: [],
    };
  },
  directives: {
    ClickOutside,
  },
  created() {
    this.allPassTypes = passTypes;
    this.passType = passTypes[0].type;
  },
  methods: {
    onPassTypeClick(selectedPassType) {
      this.passType = selectedPassType;
      this.isDropdownOpen = false;
      this.$emit("passtype-set", selectedPassType);
    },
    hideDropdown() {
      this.isDropdownOpen = false;
    },
  },
};
</script>
