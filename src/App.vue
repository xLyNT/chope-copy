<script setup lang="ts">
import Form from "./components/Form.vue";
import DateInput from "./controls/DateInput.vue";
</script>

<template>
  <div class="wrapper">
<!--    <Form/>-->
    <DateInput/>
  </div>

</template>

<style scoped lang="scss">
.wrapper{
  display: flex;
  align-items: center;
  gap:20px;
  background:#fff;
  margin-top:40%;
}
</style>
