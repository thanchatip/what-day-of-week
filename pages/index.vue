<template>
  <div class="content">
    <h2 class="header">Which day of the week is today ?</h2>
    <div class="field">
      <InputGroup class="w-full md:w-[30rem]">
        <InputGroupAddon>
          <i class="pi pi-calendar"></i>
        </InputGroupAddon>
        <InputNumber v-model="day" placeholder="Day" :min="1" :max="31" />
      </InputGroup>
    </div>
    <div class="field">
      <InputGroup class="w-full md:w-[30rem]">
        <InputGroupAddon>
          <i class="pi pi-calendar"></i>
        </InputGroupAddon>
        <Select
          v-model="month"
          :options="monthOptions"
          optionLabel="label"
          opitonValue="value"
          placeholder="Month"
        />
      </InputGroup>
    </div>
    <div class="field">
      <InputGroup class="w-full md:w-[30rem]">
        <InputGroupAddon>
          <i class="pi pi-calendar"></i>
        </InputGroupAddon>
        <InputNumber v-model="year" placeholder="Year" :min="1900" />
      </InputGroup>
    </div>
    <Button label="Submit" @click="handleClickSubmit" />
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";

const day = ref();
const month = ref();
const year = ref();

const monthOptions = ref([
  { label: "January", value: 1 },
  { label: "February", value: 2 },
  { label: "March", value: 3 },
  { label: "April", value: 4 },
  { label: "May", value: 5 },
  { label: "June", value: 6 },
  { label: "July", value: 7 },
  { label: "August", value: 8 },
  { label: "September", value: 9 },
  { label: "October", value: 10 },
  { label: "November", value: 11 },
  { label: "December", value: 12 },
]);

function handleClickSubmit() {
  const dayOfWeek = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
  ];
}
</script>

<style lang="scss" scoped>
.content {
  min-width: 450px;
  min-height: 300px;
  background: #f1f1f1;
  padding: 20px 25px;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
}

.header {
  text-align: center;
  font-size: 24px;
}

.field {
  margin-bottom: 10px;
}

.input-box {
  width: 100%;
  margin-top: 8px;
}
</style>
