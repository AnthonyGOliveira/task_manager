<script setup>
import Datepicker from '../components/Datepicker.vue'
import CardComponent from '../components/CardComponent.vue'
import IconComponent from '../components/IconComponent.vue'
import { ref } from 'vue'

const selectedDate = ref(null)
const showCalendar = ref(true)
const iconOptions = ref({
  calendar: {
    icon: 'calendar_month',
    isActive: true
  },
  taskList: {
    icon: 'list',
    isActive: false
  },
  addTask: {
    icon: 'add_task',
    isActive: false
  },
})


function handleSelectedDate(event) {
  selectedDate.value = event
  disableCalendar()
  activateIconOption(iconOptions.value['taskList'])
}

function handleShowCalendar() {
  showCalendar.value = true
  activateIconOption(iconOptions.value['calendar'])
}

function handleShowTaskList() {
  disableCalendar()
  activateIconOption(iconOptions.value['taskList'])
}

function handleShowAddTask() {
  disableCalendar()
  activateIconOption(iconOptions.value['addTask'])
}

function disableCalendar() {
  showCalendar.value = false
}

function activateIconOption(option) {
  for (let key in iconOptions.value) {
    iconOptions.value[key].isActive = iconOptions.value[key].icon === option.icon
  }
}
</script>

<template>
  <div class="teste">
    <nav class="nav-options">
      <IconComponent :icon="iconOptions['calendar'].icon" :active="iconOptions['calendar'].isActive"
        @click="handleShowCalendar" />
      <IconComponent :icon="iconOptions['taskList'].icon" :active="iconOptions['taskList'].isActive"
        @click="handleShowTaskList" />
      <IconComponent :icon="iconOptions['addTask'].icon" :active="iconOptions['addTask'].isActive"
        @click="handleShowAddTask" />
    </nav>
    <div v-show="showCalendar" class="container">
      <Datepicker @selectedDate="handleSelectedDate" />
    </div>
    <div v-show="!showCalendar" class="container">
      <CardComponent color="default">
        {{ selectedDate }}
      </CardComponent>
    </div>
  </div>
</template>

<style>
@media (min-width: 1024px) {
  .teste {
    align-items: center;
    flex-direction: row;
    justify-content: space-around;
    display: flex;
  }
}

.container {
  display: flex;
  justify-content: center;
}

.nav-options {
  display: flex;
  justify-content: space-around;
  margin: 20px 0;
}
</style>
