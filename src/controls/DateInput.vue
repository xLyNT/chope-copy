<script setup lang="ts">

import {computed, onMounted, onUnmounted, ref,useTemplateRef} from "vue";

const months = ['January','February','March','April','May','June','July','August','September','October','November','December']

const currMonth = ref(new Date().getMonth());
const currYear = ref(new Date().getFullYear());

const showCalendar = ref(false);
const containerRef =  useTemplateRef('containerRef');


function getDatesForMonth(m:number){
  const today = new Date(new Date().getFullYear(),m);
  const startDate = new Date(today.getFullYear(), today.getMonth(), 1);
  const startDay = startDate.getDay() + 1;
  const endDate = new Date(today.getFullYear(), today.getMonth() + 1, 0);
  const month = [];
  if(startDay !== 7){
    for(let i = 0; i < startDay; i++){
      month.push({
        date:0,
        isToday:false,
        day:0
      })
    }
  }

  for(let i = startDate.getDate(); i <= endDate.getDate(); i++){
    month.push({
      date:i,
      isToday:i === today.getDate(),
      day:new Date(today.getFullYear(),today.getMonth(),i).getDay()
    })
  }
  return month;
}

const month = computed(() => {
  return getDatesForMonth(currMonth.value);
});

function handlePrevMonth(){
  console.debug('TEST-',currYear.value,currMonth.value)
  if(currMonth.value === 0){
    currYear.value--;
    currMonth.value = 11;
  }else{
    currMonth.value--;
  }
}
function handleNextMonth(){
  console.debug('TEST+',currYear.value,currMonth.value)
  if(currMonth.value === 11){
    currYear.value++;
    currMonth.value = 0;
  }else{
    currMonth.value++;
  }

}

const handleClickOutside = (event:Event) => {
  //@ts-ignore
  if (containerRef.value && !containerRef.value.contains(event.target)) {
    showCalendar.value = false;
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
<div class="datepicker-wrapper" ref="containerRef">
  <div class="calendar-container" v-if="showCalendar">
    <div class="calendar">
      <div class="header">
        <div role="button" class="vc-arrow is-left" @click="handlePrevMonth">
          <svg data-v-63f7b5ec="" width="26px" height="26px" viewBox="0 -1 16 34" class="vc-svg-icon"><path data-v-63f7b5ec="" d="M11.196 10c0 0.143-0.071 0.304-0.179 0.411l-7.018 7.018 7.018 7.018c0.107 0.107 0.179 0.268 0.179 0.411s-0.071 0.304-0.179 0.411l-0.893 0.893c-0.107 0.107-0.268 0.179-0.411 0.179s-0.304-0.071-0.411-0.179l-8.321-8.321c-0.107-0.107-0.179-0.268-0.179-0.411s0.071-0.304 0.179-0.411l8.321-8.321c0.107-0.107 0.268-0.179 0.411-0.179s0.304 0.071 0.411 0.179l0.893 0.893c0.107 0.107 0.179 0.25 0.179 0.411z"></path></svg></div>
        <div class="title">
          {{ months[currMonth] + ' ' +  currYear}}
        </div>
        <div role="button" class="vc-arrow is-right" @click="handleNextMonth">
          <svg data-v-63f7b5ec="" width="26px" height="26px" viewBox="-5 -1 16 34" class="vc-svg-icon"><path data-v-63f7b5ec="" d="M10.625 17.429c0 0.143-0.071 0.304-0.179 0.411l-8.321 8.321c-0.107 0.107-0.268 0.179-0.411 0.179s-0.304-0.071-0.411-0.179l-0.893-0.893c-0.107-0.107-0.179-0.25-0.179-0.411 0-0.143 0.071-0.304 0.179-0.411l7.018-7.018-7.018-7.018c-0.107-0.107-0.179-0.268-0.179-0.411s0.071-0.304 0.179-0.411l0.893-0.893c0.107-0.107 0.268-0.179 0.411-0.179s0.304 0.071 0.411 0.179l8.321 8.321c0.107 0.107 0.179 0.268 0.179 0.411z"></path></svg></div>
      </div>
      <div class="dates">
        <div class="weekday head">S</div>
        <div class="weekday head">M</div>
        <div class="weekday head">T</div>
        <div class="weekday head">W</div>
        <div class="weekday head">T</div>
        <div class="weekday head">F</div>
        <div class="weekday head">S</div>
        <div class="weekday" v-for="day in month" :class="(day.date === 0 ? 'hide' : '') + ' ' + ((currMonth === new Date().getMonth()) && day.date < new Date().getDate() ? 'off' : '')">{{ day.date }}</div>
      </div>
    </div>
    <button type="button">
      <span>Today</span>
    </button>
  </div>
  <div class="input-container" @click="showCalendar = true">
    <input type="text" class="date-input"/>
  </div>
</div>
</template>

<style scoped lang="scss">
*{
  box-sizing: border-box;
}
.datepicker-wrapper{
  position:relative;
  height:33px;
  .calendar-container{
    display: flex;
    flex-direction: column;
    position:absolute;
    background:#fff;
    color:#000;
    width:250px;
    -webkit-box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1);
    top:34px;
    left:0;
    .calendar{

      .header{
        display:flex;
        justify-content: space-evenly;
        align-items: center;
        .vc-arrow{
          display: flex;
          justify-content: center;
          align-items: center;
          cursor: pointer;
          -webkit-user-select: none;
          user-select: none;
          pointer-events: auto;
          color: #718096;
          border-width: 2px;
          border-style: solid;
          border-radius: 0.25rem;
          border-color: transparent;
        }
      }
      .dates{
        display:grid;
        grid-template-columns: repeat(7, 1fr);
        position: relative;
        -webkit-overflow-scrolling: touch;
        padding: 5px;
        min-width: 250px;

        .weekday{
          &:not(.head){
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 14px;
            font-weight: 700;
            width: 28px;
            height: 28px;
            line-height: 28px;
            border-radius: 9999px;
            -webkit-user-select: none;
            user-select: none;
            cursor: pointer;
            &:hover{
              background-color: rgba(204, 214, 224, .3);
            }
          }
          &.head{
            text-align: center;
            color: #a0aec0;
            font-size: 14px;
            font-weight: 700;
            line-height: 14px;
            padding-top: 4px;
            padding-bottom: 8px;
            cursor: default;
            -webkit-user-select: none;
            user-select: none;
          }
          &.hide{
            opacity: 0;
          }
          &.off{
            color:#cbd5e0;
          }
        }
      }
    }
  }
}
</style>