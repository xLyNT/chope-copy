<template>
  <div class="phone-input">
    <div class="input-group">
      <div class="select-code">
        <div @click="toggleDropdown" class="selected-code">
          <img :src="selectedFlag" alt="" class="flag" />
          <span>{{ selectedCode }}</span>
          <i class="arrow" :class="{ 'arrow-up': isDropdownVisible, 'arrow-down': !isDropdownVisible }"></i>
        </div>
        <ul v-if="isDropdownVisible" class="dropdown">
          <li
              v-for="(country, index) in countries"
              :key="index"
              @click="selectCountry(country)"
              class="dropdown-item"
          >
            <img :src="country.flag" alt="" class="flag" />
            <span>{{ country.code }}</span>
          </li>
        </ul>
      </div>
      <input
          v-model="phoneNumber"
          type="text"
          class="phone-number-input"
          placeholder="Phone Number"
      />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      phoneNumber: '',
      isDropdownVisible: false,
      selectedCode: '+62',
      selectedFlag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_62.png?date=20190429',
      countries: [
        { code: '+65', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_65.png?date=20190429' },
        { code: '+81', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_81.png?date=20190429' },
        { code: '+82', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_82.png?date=20190429' },
        { code: '+86', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_86.png?date=20190429' },
        { code: '+852', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_852.png?date=20190429' },
        { code: '+44', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_44.png?date=20190429' },
        { code: '+66', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_66.png?date=20190429' },
        { code: '+62', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_62.png?date=20190429' },
        { code: '+60', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_60.png?date=20190429' },
        { code: '+886', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_886.png?date=20190429' },
        { code: '+61', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_61.png?date=20190429' },
        { code: '+1', flag: 'https://static.chope.co/static/widget_v5/img/phone_ccode_1.png?date=20190429' }
      ]
    };
  },
  methods: {
    toggleDropdown() {
      this.isDropdownVisible = !this.isDropdownVisible;
    },
    selectCountry(country) {
      this.selectedCode = country.code;
      this.selectedFlag = country.flag;
      this.isDropdownVisible = false;
    }
  }
};
</script>

<style scoped>
.phone-input {
  display: flex;
  align-items: center;
}

.input-group {
  display: flex;
  align-items: center;
  border: 1px solid #ccc;
  border-radius: 4px;
  overflow: hidden;
  color:#000;
}

.select-code {
  position: relative;
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 0 10px;
  background-color: #f5f5f5;
}

.selected-code {
  display: flex;
  align-items: center;
}

.flag {
  width: 20px;
  margin-right: 8px;
}

.arrow {
  margin-left: 8px;
}

.arrow-up {
  transform: rotate(180deg);
}

.phone-number-input {
  flex: 1;
  padding: 10px;
  border: none;
  outline: none;
}

.dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  background-color: #fff;
  border: 1px solid #ccc;
  z-index: 10;
  max-height: 200px;
  overflow-y: auto;
}

.dropdown-item {
  display: flex;
  align-items: center;
  padding: 10px;
  cursor: pointer;
}

.dropdown-item:hover {
  background-color: #f0f0f0;
}

</style>