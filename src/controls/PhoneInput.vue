<script setup lang="ts">
import {ref, onMounted, onUnmounted, useTemplateRef} from "vue";
import phoneIcon from '../assets/phoneIcon.png';

const emit = defineEmits(['update:modelValue'])

const countries = [
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
];
const phoneNumber = ref('');
const selectedFlag = ref('https://static.chope.co/static/widget_v5/img/phone_ccode_62.png?date=20190429');
const selectedCode = ref('+62');
const isDropdownVisible = ref(false);

function toggleDropdown(){
  isDropdownVisible.value = !isDropdownVisible.value;
}
function handleSelectorUpdate(country:{flag:string,code:string}) {
  selectedFlag.value = country.flag;
  selectedCode.value = country.code;
  emit('update:modelValue',selectedCode.value + phoneNumber.value);
  isDropdownVisible.value = false;
}

function handleInputUpdate(){
  emit('update:modelValue',selectedCode.value + phoneNumber.value);
}

const containerRef = useTemplateRef('containerRef');
const handleClickOutside = (event:Event) => {
  //@ts-ignore
  if (containerRef.value && !containerRef.value.contains(event.target)) {
    isDropdownVisible.value = false;
  }
}
onMounted(() => {
  document.addEventListener("click", handleClickOutside)
})

onUnmounted(() => {
  document.removeEventListener("click", handleClickOutside)
})
</script>

<template>
  <div class="phone-input" ref="containerRef">
    <div class="input-group">
      <img class="icon" :src="phoneIcon" alt=""/>
      <div class="select-code">
        <div @click="toggleDropdown" class="selected-code">
          <img :src="selectedFlag" alt="" class="flag" />
          <span>{{ selectedCode }}</span>
          <span class="el-input__suffix">
            <span class="el-input__suffix-inner">
              <i class="arrow el-icon-inner" :class="{ 'arrow-up': isDropdownVisible, 'arrow-down': !isDropdownVisible }"></i>
            </span>
          </span>
        </div>
        <div class="dropdown-wrapper" v-show="isDropdownVisible">
          <ul class="dropdown">
            <li
                v-for="(country, index) in countries"
                :key="index"
                @mousedown="handleSelectorUpdate(country)"
                class="dropdown-item"
            >
              <img :src="country.flag" alt="" class="flag" />
              <span>{{ country.code }}</span>
            </li>
          </ul>
          <div class="popper__arrow"></div>
        </div>

      </div>
      <input
          v-model="phoneNumber"
          @input="handleInputUpdate"
          type="tel"
          class="input"
          placeholder="Phone Number"
      />
    </div>
  </div>
</template>

<style scoped lang="scss">
.phone-input{
  width:100%;
  .input-group{
    display: flex;
    align-items: center;
    gap:10px;
    border: 1px solid #d4d4d4;
    border-radius: 4px;
    padding:0 10px;
    width: 100%;
    height: 100%;
    color:#000;
    .input{
      background:#fff;
      width:100%;
      height: 100%;
      border:0;
      color:#000;
      &:focus{
        outline:none;
      }
    }
    .icon{
      width:20px;
    }
    .select-code{
      position: relative;
      border: none;
      background: transparent;
      color: #909399;
      vertical-align: middle;
      display: table-cell;
      border-radius: 4px;
      white-space: nowrap;

      .selected-code{
        display:flex;
        align-items: center;
        gap:10px;
        padding:0 20px 0 10px;
        width:100px;
      }
      .el-input__suffix{
        position: absolute;
        top:-3px;
        right:-5px;
        -webkit-transition: all .3s;
        transition: all .3s;
        pointer-events: none;
        height: 100%;
        color: #c0c4cc;
        text-align: center;
        .el-input__suffix-inner{
          margin-top: -4px;
          display: inline-block;
          pointer-events: all;
          .arrow{
            height: 100%;
            width: 25px;
            text-align: center;
            line-height: 40px;
            color: #c0c4cc;
            font-size: 14px;
            -webkit-transition: -webkit-transform .3s;
            transition: -webkit-transform .3s;
            transition: transform .3s;
            transition: transform .3s, -webkit-transform .3s;
            cursor: pointer;
            &:before {
              content: "\e6e1";
            }
            &:after{
              content: "";
              height: 100%;
              width: 0;
              display: inline-block;
              vertical-align: middle;
            }
            &.arrow-down{
              -webkit-transform: rotate(180deg);
              transform: rotate(180deg);
            }
          }
        }

      }
      .dropdown-wrapper{
        width:100%;
        max-height:200px;
        overflow-y:auto;
        overflow-x:hidden;
        position: absolute;
        top:40px;
        scrollbar-width: thin;
        scrollbar-color: hsla(220, 4%, 58%, .3) #fff;
        z-index: 1001;
        border: 1px solid #e4e7ed;
        border-radius: 4px;
        -webkit-box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1);
        box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1);
        margin-right:-15px;

        &::-webkit-scrollbar {
          height: 12px;
          width: 12px;
          background: #FFF;
        }
        &::-webkit-scrollbar-track {
          background: transparent;
          -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
        }
        &::-webkit-scrollbar-thumb {
          background-color: hsla(220, 4%, 58%, .3);
          border-radius: 5px;
          border: 3px solid transparent;
          -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
        }

        .dropdown{
          list-style:none;
          background:#fff;
          padding: 6px 0;
          margin-right:-15px;
          .dropdown-item{
            display: flex;
            align-items: center;
            gap:8px;
            cursor:pointer;
            font-size: 14px;
            padding: 0 35px 0 10px;
            position: relative;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            color: #606266;
            height: 34px;
            line-height: 34px;

            &:hover{
              background-color: #f5f7fa;
            }
          }
        }
        .popper__arrow{
          top: -6px;
          left: 50%;
          margin-right: 3px;
          border-bottom-color: #ebeef5;
          border-width: 6px;
          border-top-width: 0;
          -webkit-filter: drop-shadow(0 2px 12px rgba(0, 0, 0, .03));
          filter: drop-shadow(0 2px 12px rgba(0, 0, 0, .03));
          &:after{
            position: absolute;
            display: block;
            width: 0;
            height: 0;
            border-color: transparent;
            border-style: solid;
            content: " ";
            border-width: 6px;
            top: 1px;
            margin-left: -6px;
            border-top-width: 0;
            border-bottom-color: #fff;
          }
        }
      }
    }
  }
}

</style>