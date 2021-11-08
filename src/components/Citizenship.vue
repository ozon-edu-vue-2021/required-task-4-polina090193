<template>
  <div class="citizenship-selector" v-click-outside="hideDropdown">
    <label class="label label_choose" :for="selectorType">{{ getTitle }}</label>
    <input
      v-model="citizenship"
      :id="selectorType"
      @focus="isDropdownOpen = true"
      class="input input_choose"
    />
    <div
      class="clear-citizenship"
      v-if="!!citizenship"
      @click="clearCitizenship"
    >
      ×
    </div>
    <div v-if="isDropdownOpen" class="citizenship-selector___dropdown">
      <ul v-if="allCitizenships.length">
        <li
          v-for="citizenship in allCitizenships"
          :key="citizenship.id"
          @click="onCitizenshipClick(citizenship.nationality)"
        >
          {{ citizenship.flag }} {{ citizenship.nationality }}
        </li>
      </ul>
      <div v-else class="empty">Ничего не найдено</div>
    </div>
  </div>
</template>

<script>
import ClickOutside from "vue-click-outside";
import citizenships from "../assets/data/citizenships.json";
import { debounce } from "../helpers/debounce";

export default {
  props: {
    selectorType: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      isDropdownOpen: false,
      citizenship: null,
      allCitizenships: [],
      debouncedSearchCitizenships: null,
    };
  },
  computed: {
    getTitle() {
      return this.selectorType === "citizen"
        ? "Гражданство*"
        : this.selectorType === "issue-country"
        ? "Страна выдачи*"
        : "";
    },
  },
  directives: {
    ClickOutside,
  },
  created() {
    this.allCitizenships = citizenships;
    this.debouncedSearchCitizenships = debounce(this.getCitizenships, 500);
  },
  watch: {
    citizenship(newValue) {
      if (this.selectorType === "citizen") {
        !newValue
          ? this.noCitizenShipSelected()
          : this.debouncedSearchCitizenships(newValue);
      } else if (this.selectorType === "issue-country" && !newValue) {
        this.noCountrySelected();
      }
    },
  },
  methods: {
    onCitizenshipClick(selectedCitizenship) {
      this.citizenship = selectedCitizenship;
      this.isDropdownOpen = false;
      if (this.selectorType === "citizen") {
        this.$emit("citizenship-set", selectedCitizenship);
      } else if (this.selectorType === "issue-country") {
        this.$emit("country-set", selectedCitizenship);
      }
    },
    hideDropdown() {
      this.isDropdownOpen = false;
    },
    getCitizenships(searchWord) {
      this.allCitizenships = citizenships.filter((citizenship) =>
        citizenship.nationality.toLowerCase().includes(searchWord.toLowerCase())
      );
    },
    clearCitizenship() {
      this.citizenship = "";
      this.allCitizenships = citizenships;
      if (this.selectorType === "citizen") {
        this.noCitizenShipSelected();
      } else if (this.selectorType === "issue-country") {
        this.noCountrySelected();
      }
    },
    noCitizenShipSelected() {
      this.$emit("no-citizenship");
    },
    noCountrySelected() {
      this.$emit("no-country");
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
