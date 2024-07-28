<template>
  <div class="content">
    <h2 class="header">Which day of the week is today ?</h2>
    <div class="field">
      <InputGroup class="w-full md:w-[30rem]">
        <InputGroupAddon>
          <i class="pi pi-calendar"></i>
        </InputGroupAddon>
        <InputNumber v-model="day" placeholder="Day" :min="1" :max="31" />
        <Select
          v-model="month"
          :options="monthOptions"
          optionLabel="label"
          opitonValue="value"
          placeholder="Month"
        />
        <InputNumber
          v-model="year"
          placeholder="Year"
          :min="1900"
          :useGrouping="false"
        />
      </InputGroup>
    </div>
    <Dialog
      class="dialog-box"
      v-model:visible="visible"
      modal
      header="Day of the Week"
    >
      <span class="dialog-text">
        {{ day }} {{ month.label }} {{ year }} is
        {{ findDayOfWeek() }}
      </span>

      <div class="dialog-button">
        <Button class="button" label="OK" @click="toggleVisible"></Button>
      </div>
    </Dialog>
    <Button
      class="button"
      label="Submit"
      :disabled="isDisabledSubmitButton"
      @click="handleClickSubmit"
    />
  </div>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import { useDialog } from "primevue/usedialog";

const day = ref();
const month = ref();
const year = ref();
const visible = ref(false);

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

const isDisabledSubmitButton = computed(() => {
  if (day.value) {
    if (month.value) {
      if (year.value) {
        return false;
      }
    }
  }
  return true;
});

function handleClickSubmit() {
  toggleVisible();
}

function toggleVisible() {
  visible.value = !visible.value;
}

function isLeapYear(year: number) {
  // A year is leap if it's divisible by 4, but not by 100 unless it's also divisible by 400.
  if (year % 4 === 0) {
    if (year % 100 === 0) {
      if (year % 400 === 0) {
        return true;
      }
    } else {
      return true;
    }
  } else {
    return false;
  }
}

function getDateAmountFromDateAndYear(month: number, year: number) {
  // Calculate the number of days in the given month and year.
  const daysInMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];

  if (isLeapYear(year) && month === 2) {
    daysInMonth[1] = 29;
  }

  return daysInMonth[month - 1];
}

function calculateDaysFromInputDate(day: number, month: number, year: number) {
  // Calculate the number of days from the given date to January 1st of the same year.
  let totalDaysFromJan1 = 0;

  for (let i = 1900; i < year; i++) {
    totalDaysFromJan1 += isLeapYear(i) ? 366 : 365;
  }

  for (let j = 1; j < month; j++) {
    totalDaysFromJan1 += getDateAmountFromDateAndYear(j, year);
  }

  totalDaysFromJan1 += day;

  return totalDaysFromJan1;
}

function findDayOfWeek() {
  const dayOfWeek = [
    "Sunday",
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday",
    "Saturday",
  ];

  const totalDays = calculateDaysFromInputDate(
    day.value,
    month.value.value,
    year.value
  );

  return dayOfWeek[totalDays % 7];
}
</script>

<style lang="scss" scoped>
.content {
  min-width: 450px;
  background: #f3f4f6;
  padding: 25px 35px;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  box-shadow: rgba(9, 30, 66, 0.25) 0px 4px 8px -2px,
    rgba(9, 30, 66, 0.08) 0px 0px 0px 1px;
}

.header {
  text-align: center;
  font-size: 24px;
}

.field {
  margin-bottom: 10px;
}

.button {
  margin-top: 20px;
}

.dialog-text {
  text-align: center;
  font-size: 20px;
}

.dialog-button {
  margin-top: 10px;
  display: flex;
  justify-content: right;
}
</style>
