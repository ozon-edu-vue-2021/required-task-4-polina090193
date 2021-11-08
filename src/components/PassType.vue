<template>
  <div class="passtype-selector" v-click-outside="hideDropdown">
    <label class="label label_choose" for="pass-type">Тип паспорта*</label>
    <input
      v-model="passType"
      id="pass-type"
      @focus="isDropdownOpen = true"
      class="input input_choose"
    />
    <div v-if="isDropdownOpen" class="passtype-selector___dropdown">
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
  },
  methods: {
    onPassTypeClick(selectedPassType) {
      this.passType = selectedPassType;
      this.isDropdownOpen = false;
    },
    hideDropdown() {
      this.isDropdownOpen = false;
    },
  },
};
</script>

<style scoped>
.citizenship-selector {
  position: relative;
}
.citizenship-selector___dropdown {
  position: absolute;
  top: calc(100% - 14px);
  max-height: 200px;
  overflow-y: auto;
  width: 100%;
  background: white;
  border-radius: 10px;
  color: rgb(37, 37, 41);
}
.citizenship-selector___dropdown ul {
  padding-left: 0;
}
.citizenship-selector___dropdown li,
.empty {
  padding: 4px 10px;
  list-style: none;
  cursor: pointer;
}
.citizenship-selector___dropdown li:hover {
  background-color: rgb(37, 37, 41);
  color: lightgrey;
}
.clear-citizenship {
  position: absolute;
  top: 33%;
  right: 10px;
  font-size: 30px;
  cursor: pointer;
  color: rgb(37, 37, 41);
}
</style>
