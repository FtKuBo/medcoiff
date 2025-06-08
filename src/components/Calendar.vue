<template>
  <div class="calendar-container">
    <div class="calendar-header">
      <h2>{{ currentMonthName }} {{ currentYear }}</h2>
      <div class="calendar-nav">
        <button @click="previousMonth" class="nav-btn">&lt;</button>
        <button @click="nextMonth" class="nav-btn">&gt;</button>
      </div>
    </div>
    <div class="calendar-grid">
      <div class="weekdays">
        <div v-for="day in weekDays" :key="day">{{ day }}</div>
      </div>
      <div class="days">
        <div
          v-for="day in calendarDays"
          :key="day.date"
          :class="[
            'day',
            { 'other-month': !day.currentMonth },
            { 'available': isAvailable(day.date) },
            { 'selected': isSelected(day.date) }
          ]"
          @click="selectDate(day.date)"
        >
          {{ day.dayNumber }}
        </div>
      </div>
    </div>
    <div class="time-slots" v-if="selectedDate">
      <h3>Time Slots for {{ formatSelectedDate }}</h3>
      <div class="slots-grid">
        <button
          v-for="slot in availableTimeSlots"
          :key="slot"
          :class="{ 'selected': selectedTimeSlot === slot }"
          @click="selectTimeSlot(slot)"
          class="time-slot-btn"
        >
          {{ slot }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch } from 'vue'

const props = defineProps<{
  resetTrigger?: number
}>()

const weekDays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
const currentDate = ref(new Date())
const selectedDate = ref<Date | null>(null)
const selectedTimeSlot = ref<string | null>(null)

// Watch for reset trigger
watch(() => props.resetTrigger, () => {
  selectedDate.value = null
  selectedTimeSlot.value = null
})

// Generate availability data for current month and next 3 months
const generateAvailabilityData = () => {
  const data: Record<string, string[]> = {}
  const today = new Date()
  const endDate = new Date()
  endDate.setMonth(today.getMonth() + 3) // Generate for next 3 months

  const weekdayTimeSlots = ['09:00', '10:00', '11:00', '14:00', '15:00', '16:00']
  const saturdayTimeSlots = ['09:00', '10:00', '11:00', '12:00', '13:00'] // Shorter hours on Saturday
  
  while (today <= endDate) {
    // Skip only Sundays
    if (today.getDay() !== 0) {
      const dateKey = today.toISOString().split('T')[0]
      const isSaturday = today.getDay() === 6
      const availableSlots = isSaturday 
        ? saturdayTimeSlots.sort(() => Math.random() - 0.5).slice(0, Math.floor(Math.random() * 2) + 3) // 3-4 slots for Saturday
        : weekdayTimeSlots.sort(() => Math.random() - 0.5).slice(0, Math.floor(Math.random() * 3) + 3) // 3-5 slots for weekdays
      data[dateKey] = availableSlots
    }
    today.setDate(today.getDate() + 1)
  }
  return data
}

const availabilityData = generateAvailabilityData()

const currentMonthName = computed(() => {
  return currentDate.value.toLocaleString('default', { month: 'long' })
})

const currentYear = computed(() => {
  return currentDate.value.getFullYear()
})

const formatSelectedDate = computed(() => {
  if (!selectedDate.value) return ''
  return selectedDate.value.toLocaleDateString('en-US', {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  })
})

const calendarDays = computed(() => {
  const year = currentDate.value.getFullYear()
  const month = currentDate.value.getMonth()
  
  const firstDay = new Date(year, month, 1)
  const lastDay = new Date(year, month + 1, 0)
  
  const days = []
  
  // Add days from previous month
  const firstDayOfWeek = firstDay.getDay()
  for (let i = firstDayOfWeek - 1; i >= 0; i--) {
    const date = new Date(year, month, -i)
    days.push({
      date,
      dayNumber: date.getDate(),
      currentMonth: false
    })
  }
  
  // Add days from current month
  for (let i = 1; i <= lastDay.getDate(); i++) {
    const date = new Date(year, month, i)
    days.push({
      date,
      dayNumber: i,
      currentMonth: true
    })
  }
  
  // Add days from next month
  const remainingDays = 42 - days.length // 6 rows * 7 days
  for (let i = 1; i <= remainingDays; i++) {
    const date = new Date(year, month + 1, i)
    days.push({
      date,
      dayNumber: date.getDate(),
      currentMonth: false
    })
  }
  
  return days
})

const availableTimeSlots = computed(() => {
  if (!selectedDate.value) return []
  const dateKey = selectedDate.value.toISOString().split('T')[0]
  return availabilityData[dateKey] || []
})

function previousMonth() {
  currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() - 1, 1)
}

function nextMonth() {
  currentDate.value = new Date(currentDate.value.getFullYear(), currentDate.value.getMonth() + 1, 1)
}

function isAvailable(date: Date) {
  const dateKey = date.toISOString().split('T')[0]
  return availabilityData[dateKey] !== undefined
}

function isSelected(date: Date) {
  if (!selectedDate.value) return false
  return date.toDateString() === selectedDate.value.toDateString()
}

function selectDate(date: Date) {
  if (isAvailable(date)) {
    selectedDate.value = date
    selectedTimeSlot.value = null
  }
}

function selectTimeSlot(slot: string) {
  selectedTimeSlot.value = slot
}

// Emit selected date and time slot
const emit = defineEmits(['date-selected'])

watch([selectedDate, selectedTimeSlot], ([date, slot]) => {
  if (date && slot) {
    emit('date-selected', {
      date: date.toISOString().split('T')[0],
      timeSlot: slot
    })
  }
})
</script>

<style scoped>
.calendar-container {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.calendar-nav {
  display: flex;
  gap: 10px;
}

.nav-btn {
  background: #2196f3;
  color: white;
  border: none;
  border-radius: 4px;
  padding: 8px 16px;
  cursor: pointer;
  font-size: 1.2em;
  transition: background-color 0.2s;
}

.nav-btn:hover {
  background: #1976d2;
}

.calendar-grid {
  display: grid;
  gap: 5px;
}

.weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
  font-weight: bold;
  margin-bottom: 10px;
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 5px;
}

.day {
  aspect-ratio: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  border-radius: 4px;
  transition: all 0.2s;
  font-size: 1.1em;
  border: 1px solid #eee;
}

.day:hover {
  background-color: #f0f0f0;
  transform: scale(1.05);
}

.other-month {
  color: #ccc;
  background-color: #f9f9f9;
}

.available {
  background-color: #e3f2fd;
  border-color: #2196f3;
}

.selected {
  background-color: #2196f3;
  color: white;
  border-color: #1976d2;
}

.time-slots {
  margin-top: 20px;
  padding-top: 20px;
  border-top: 1px solid #eee;
}

.slots-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
  gap: 10px;
  margin-top: 10px;
}

.time-slot-btn {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
  color: black;
  background: white;
  cursor: pointer;
  transition: all 0.2s;
  font-size: 1em;
}

.time-slot-btn:hover {
  background-color: #f0f0f0;
  transform: scale(1.05);
}

.time-slot-btn.selected {
  background-color: #2196f3;
  color: white;
  border-color: #1976d2;
}
</style> 
