<script setup>
import { onMounted, ref, computed, defineEmits } from 'vue'

const daysOfWeek = [0, 1, 2, 3, 4, 5, 6]
const numberOfWeeks = [0, 1, 2, 3, 4, 5]
const initialDay = ref(1);
const initialDayOfWeek = ref(null);
const numberDaysOfMonth = ref(null);
const calendar = ref(null)
const selectedDate = ref(null)
const currentDate = ref(null)
const selectedDay = ref(null)
const selectedSpecificDate = ref(null)
const emit = defineEmits(['selectedDate'])

onMounted(() => {
    selectedDate.value = new Date()
    currentDate.value = new Date()
    generateCalendar(selectedDate.value)
})

function getDaysInCurrentMonth(dateValue) {
    const year = getYear(dateValue)
    const month = getMonth(dateValue)
    return new Date(year, month + 1, 0).getDate();
}

function getYear(dateValue) {
    if (!dateValue) {
        return null
    }
    return dateValue.getFullYear();
}

function getMonth(dateValue) {
    if (!dateValue) {
        return null
    }
    return dateValue.getMonth();
}

function getMonthText(dateValue) {
    const month = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
    return month[getMonth(dateValue)]
}

function getFirstDayOfWeekInCurrentMonth(dateValue) {
    const year = dateValue.getFullYear();
    const month = dateValue.getMonth();
    const firstDay = new Date(year, month, 1).getDay();
    return firstDay
}

function incrementDay(week, dayOfWeek) {
    let day = '';
    if (initialDay.value > 1 && initialDay.value <= numberDaysOfMonth.value) {
        day = initialDay.value
        initialDay.value += 1
    }
    else if (initialDay.value === 1 && week === 0 && dayOfWeek === initialDayOfWeek.value) {
        day = initialDay.value
        initialDay.value += 1
    }
    return day
}

function generateCalendar(dateValue) {
    calendar.value = null
    initialDay.value = 1
    numberDaysOfMonth.value = getDaysInCurrentMonth(dateValue)
    initialDayOfWeek.value = getFirstDayOfWeekInCurrentMonth(dateValue)
    calendar.value = []
    numberOfWeeks.forEach(week => {
        calendar.value[week] = []
        daysOfWeek.forEach(dayOfWeek => {
            const day = incrementDay(week, dayOfWeek)
            calendar.value[week][dayOfWeek] = day
        })
    });
}

function selectDay(event) {
    if (event.target.textContent) {
        console.log('event: ', event);
        selectedDay.value = event.target.textContent
        const dateToEmit = new Date(selectedDate.value)
        selectedSpecificDate.value = new Date(dateToEmit.setDate(Number(selectedDay.value)))
        selectedSpecificDate.value = setTimeInDate(selectedSpecificDate.value)
        emit('selectedDate', selectedSpecificDate.value)
    }
}

function setTimeInDate(oldDate) {
    const newDate = new Date()
    let currentHours = newDate.getHours();
    let currentMinutes = newDate.getMinutes();
    let currentSeconds = newDate.getSeconds();
    let currentMilliseconds = newDate.getMilliseconds();

    // Atualize o tempo do objeto Date antigo para o tempo atual
    oldDate.setHours(currentHours);
    oldDate.setMinutes(currentMinutes);
    oldDate.setSeconds(currentSeconds);
    oldDate.setMilliseconds(currentMilliseconds);
    return oldDate
}

function prevMonth() {
    selectedDate.value = new Date(selectedDate.value.setMonth(getMonth(selectedDate.value) - 1));
    generateCalendar(selectedDate.value)
}

function nextMonth() {
    selectedDate.value = new Date(selectedDate.value.setMonth(getMonth(selectedDate.value) + 1));
    generateCalendar(selectedDate.value)
}

const currentDay = computed(() => currentDate.value.getDate())
const isActualCurrentMonth = computed(() => getMonth(currentDate.value) === getMonth(selectedDate.value))
const isActualCurrentYear = computed(() => getYear(currentDate.value) === getYear(selectedDate.value))
const verifyActualCurrentDate = computed(() => isActualCurrentMonth.value && isActualCurrentYear.value)

const isSelectedMonth = computed(() => getMonth(selectedSpecificDate.value) === getMonth(selectedDate.value))
const isSelectedYear = computed(() => getYear(selectedSpecificDate.value) === getYear(selectedDate.value))
const verifySelectedDate = computed(() => isSelectedMonth.value && isSelectedYear.value)

const actualSelectedDay = computed(() => Number(selectedDay.value))
const selectedYear = computed(() => getYear(selectedDate.value))
const selectedMonth = computed(() => getMonthText(selectedDate.value))
const yearMonthDescription = computed(() => `${selectedYear.value} - ${selectedMonth.value}`)
</script>

<template>
    <div class="calendar-component">
        <div class="calendar-container">
            <div class="calendar-header">
                <button id="prev-month" class="calendar-button" @click="prevMonth">&lt;</button>
                <span id="month-year" class="calendar-year-month">{{ yearMonthDescription }}</span>
                <button id="next-month" class="calendar-button" @click="nextMonth">&gt;</button>
            </div>
            <table class="calendar">
                <thead>
                    <tr>
                        <th>Dom</th>
                        <th>Seg</th>
                        <th>Ter</th>
                        <th>Qua</th>
                        <th>Qui</th>
                        <th>Sex</th>
                        <th>Sáb</th>
                    </tr>
                </thead>
                <tbody v-if="calendar" id="calendar-body">
                    <tr v-for="trItem in numberOfWeeks">
                        <td v-for="tdItem in daysOfWeek" @click="selectDay" :class="{
                            'calendar-day': calendar[trItem][tdItem],
                            'calendar-day-current': currentDay === calendar[trItem][tdItem] && selectedDay !== calendar[trItem][tdItem] && verifyActualCurrentDate,
                            'calendar-day-selected': actualSelectedDay === calendar[trItem][tdItem] && verifySelectedDate
                        }">
                            {{ calendar[trItem][tdItem] }}
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>
</template>

<style scoped>
.calendar-container {
    background: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    width: 345px;
    border-radius: 20px;
    overflow: hidden;

}

.calendar-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background: #f0f0f0;
}

.calendar {
    width: 100%;
    border-collapse: collapse;
}

.calendar th,
.calendar td {
    width: 50px;
    height: 50px;
    text-align: center;
    padding: 8px;
    /* border: 1px solid #ccc; */

}

.calendar-day {
    cursor: pointer;
}

.calendar-day-current {
    background-color: rgb(136, 120, 143);
}

.calendar-day-selected {
    background-color: #1ca060;
    color: black;
    font-weight: bold;
}

.calendar-day:hover {
    background: #e0e0e0;
}

.calendar-button {
    width: 50px;
    height: 50px;
    font-weight: bold;
    font-size: 30px;
    border: none;
    background-color: #f0f0f0;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #95948C;
}

.calendar-button:hover {
    background: #e0e0e0;
}

.calendar-year-month {
    font-size: 25px;
    color: #95948C;
}
</style>
