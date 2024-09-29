<script setup lang="ts">
import {computed, onMounted, onUnmounted, PropType, ref, useTemplateRef} from "vue";
import DropdownIcon from '../assets/dropdownIcon.svg'

const props = defineProps({
  list:{
    required:true,
    type:Object as PropType<any[]>
  },
  image:{
    required:true,
    type:String
  }
});

const emit = defineEmits(['select']);
const isChanged = ref(false);
const showList = ref(false);
const inputVal = ref('');
const placeholderVal = ref(props.list[0].title);
const input = useTemplateRef('input');
const containerRef = useTemplateRef('containerRef');
const selected = ref('');


const filteredList = computed(() => {
  const res = props.list.filter(item => item.title.toLowerCase().includes(inputVal.value.toLowerCase()));
  return res;
})

function handleInput(){
  placeholderVal.value = inputVal.value || props.list[0].title;
  inputVal.value = '';
  input.value?.focus();
  showList.value = true;
}
function handleSelect(item:{title:string,value:string}){
  isChanged.value = true;
  inputVal.value = item.title;
  placeholderVal.value = item.title;
  selected.value = item.value;
  showList.value = false;
  emit('select',item)
}


const handleClickOutside = (event:Event) => {
  //@ts-ignore
  if (containerRef.value && !containerRef.value.contains(event.target)) {
    showList.value = false;
    if(placeholderVal.value && isChanged.value){
      inputVal.value = placeholderVal.value;
    }
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
<div class="wrapper" ref="containerRef">
  <div class="select" @click="handleInput">
    <img v-if="image" :src="image" alt=""/>
    <input type="text" :value="inputVal" autocomplete="off" :placeholder="placeholderVal" class="inner_input" ref="input" :readonly="!showList" />
    <div class="dropdown_icon">
      <img class="dropdown_icon__img" :src="DropdownIcon" alt=""/>
    </div>
  </div>
  <div class="dropdown_list" v-if="showList" @click.stop>
    <div class="dropdown_list__item" v-for="item in filteredList" @click="handleSelect(item)" :class="selected == item.value ? 'bold' : ''">{{ item.title }}</div>
    <div class="dropdown_list__empty" v-if="!filteredList.length">No matching data</div>
  </div>
</div>
</template>

<style scoped lang="scss">
*{
  box-sizing: border-box;
}
.wrapper{
  position:relative;
  width:100%;
  .select{
    display:flex;
    align-items: center;
    height: 34px;
    cursor:pointer;
    background:#fff;
    border:1px solid #d4d4d4;
    border-radius:3px;
    padding:0 5px;
    .inner_input{
      border:0;
      background:transparent;
      cursor:pointer;
      margin-left:5px;
      width:calc(100% - 34px);
      color:#000;
      &:focus{
        outline:0;
        border:0;
      }
    }
    .dropdown_icon{
      width:18px;
      height: 18px;
      display:flex;
      align-items: center;
    }
    >img{
      width:16px;
      margin:0 5px;
    }
  }
  .dropdown_list{
    width:100%;
    background-color: #fff;
    max-height: 200px;
    overflow-y: auto;
    color:#000;
    border: 1px solid #e4e7ed;
    border-radius: 4px;
    -webkit-box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1);
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, .1);
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    margin-bottom: -15px;
    margin-right: -15px;
    position: absolute;
    top:45px;
    scrollbar-width: thin;
    scrollbar-color: hsla(220, 4%, 58%, .3) transparent;
    z-index:10;

    &::-webkit-scrollbar {
      height: 12px;
      width: 12px;
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
    ::-webkit-scrollbar-button {
      height: 0;
      width: 0
    }

    .dropdown_list__item{
      max-width: 360px;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
      box-sizing: border-box;
      color: #606266;
      cursor: pointer;
      font-size: 14px;
      height: 34px;
      line-height: 34px;
      padding: 0 20px;
      position: relative;
      text-align: left;

      &.bold{
        //color: #2b2a29;
        color:rgb(96, 98, 102);
        font-weight: 700;
      }

      &:hover{
        background:#f5f7fa;
      }
    }
    .dropdown_list__empty{
      color: #999;
      font-size: 14px;
      margin: 0;
      padding: 10px 0;
      text-align: center;
    }
  }
}
</style>