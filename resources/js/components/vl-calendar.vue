<template>
  <div class="vl-calendar">
    <span
      class="vl-calendar__arrow vl-calendar__arrow--back"
      @click="moveBack()"
    />

    <span
      class="vl-calendar__arrow vl-calendar__arrow--forward"
      @click="moveForward()"
    />

    <div class="calendar_wrap">

        <vl-calendar-month
            class="vl-calendar__month"
            :month="currentMonthMonth"
            :year="currentMonthYear"
            :is-selected="isSelected"
            :is-disabled="isDisabled"
            :custom-classes="customClasses"
            :show-weeks-number="showWeeksNumber"
            @input="date => $emit('input', date)"
        />

        <vl-calendar-month
            v-if="!singleMonth"
            class="vl-calendar__month"
            :month="nextMonthMonth"
            :year="nextMonthYear"
            :is-selected="isSelected"
            :is-disabled="isDisabled"
            :custom-classes="customClasses"
            :show-weeks-number="showWeeksNumber"
            @input="date => $emit('input', date)"
        />
    </div>
  </div>
</template>

<script>
import VlCalendarMonth from './vl-calendar-month'
import * as DatesUtils from '../utils/DatesUtils'

export default {
  name: 'VlCalendar',
  components: {
    VlCalendarMonth
  },

  props: {
    isSelected: Function,
    isDisabled: Function,
    customClasses: Object,
    showWeeksNumber: Boolean,
    singleMonth: Boolean,
    defaultDate: String
  },

  data () {
    const defaultDate = this.defaultDate ? DatesUtils.parseDate(this.defaultDate) : DatesUtils.getToday()
    return {
      currentMonthMonth: defaultDate.getMonth(),
      currentMonthYear: defaultDate.getFullYear()
    }
  },

  computed: {
    nextMonthMonth () {
      return this.currentMonthMonth === 11 ? 0 : this.currentMonthMonth + 1
    },

    nextMonthYear () {
      return this.currentMonthMonth === 11 ? this.currentMonthYear + 1 : this.currentMonthYear
    }
  },

  methods: {
    moveBack () {
      if (this.currentMonthMonth === 0) {
        this.currentMonthMonth = 11
        this.currentMonthYear--
      } else {
        this.currentMonthMonth--
      }
    },

    moveForward () {
      if (this.currentMonthMonth === 11) {
        this.currentMonthMonth = 0
        this.currentMonthYear++
      } else {
        this.currentMonthMonth++
      }
    }
  }
}
</script>
