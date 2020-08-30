<template>
  <div>
    <div v-if="visible" class="datepicker border shadow flex" @click.stop>
      <div class="datepicker__options bg-blue-400">
        <div class="datepicker__header bg-blue-400 p-4">
          <div class="datepicker__year">{{ year }}</div>
          <div class="datepicker__date">
            {{ date.format('dddd DD MMMM') }}
          </div>
        </div>
      </div>
      <div class="datepicker__calendar">
        <div class="datepicker__controls flex items-center justify-between">
          <div class="datepicker__month">{{ month }}</div>
          <button
            class="datepicker__prev focus:outline-none"
            @click="prevMonth()"
          >
            <img
              :src="require('../assets/img/chevron-left-solid.svg')"
              alt="Previous"
            />
          </button>
          <button
            class="datepicker__next focus:outline-none"
            @click="nextMonth()"
          >
            <img
              :src="require('../assets/img/chevron-right-solid.svg')"
              alt="Next"
            />
          </button>
        </div>
        <div class="datepicker__week">
          <div
            v-for="day in days"
            :key="day.index"
            class="datepicker__weekday cursor-pointer hover:text-blue-500"
            @click="selectDateByDay(day.long)"
          >
            {{ day.short }}
          </div>
        </div>
        <div class="datepicker__days">
          <div
            class="datepicker__day"
            :style="{ width: getWeekStart() * 41 + 'px' }"
          ></div>
          <div
            v-for="day in getDays()"
            :key="day.i"
            class="datepicker__day"
            :class="{ selected: isSelected(day) }"
          >
            <div v-if="firstSelect > day && secondSelect === ''">
              <span class="datepicker__day__text text-gray-300">{{
                day.format('DD')
              }}</span>
            </div>
            <div v-else @click="selectDate(day)">
              <span class="datepicker__day__effect"></span>
              <span class="datepicker__day__text">{{ day.format('DD') }}</span>
            </div>
          </div>
        </div>
        <div class="clear-both"></div>
        <div class="datepicker__actions justify-end flex p-5">
          <button
            class="px-4 py-2 hover:text-blue-500 hover:bg-gray-200 font-semibold"
            @click="cancel"
          >
            Annuler
          </button>
          <button
            class="px-4 py-2 hover:text-blue-500 hover:bg-gray-200 font-semibold"
            @click="actionSubmit"
          >
            ok
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import moment from 'moment'
import { extendMoment } from 'moment-range'
const Moment = extendMoment(moment)

export default {
  props: {
    date: { type: Object, default: null },
    visible: { type: Boolean, default: false }
  },
  data() {
    return {
      days: [
        { short: 'Lu', long: 'lundi' },
        { short: 'Ma', long: 'mardi' },
        { short: 'Me', long: 'mercredi' },
        { short: 'Je', long: 'jeudi' },
        { short: 'Ve', long: 'vendredi' },
        { short: 'Sa', long: 'samedi' },
        { short: 'Di', long: 'dimanche' }
      ],
      startMonth: this.date.clone().startOf('month'),
      endMonth: this.date.clone().endOf('month'),
      month: this.date.format('MMMM'),
      year: this.date.format('YYYY'),
      updateDate: null,
      monthList: [],
      monthListDay: [],
      selectDay: '',
      isActive: 0,
      clicked: true,
      firstSelect: '',
      firstSelected: '',
      secondSelect: ''
    }
  },
  computed: {
    year_formated() {
      return this.date.format('YYYY')
    },
    date_formated() {
      return this.date.format('dddd DD MMM')
    }
  },
  methods: {
    isSelected(day) {
      if (this.selectDay === '') {
        if (this.firstSelected) {
          if (
            this.firstSelected === day.unix() ||
            (this.firstSelected <= day.unix() &&
              this.secondSelected >= day.unix())
          )
            return true
          else {
            return false
          }
        }
      } else {
        for (const key in this.monthListDay) {
          if (this.monthListDay[key].format('dddd') === day.format('dddd'))
            return true
        }
      }
    },
    selectDate(day) {
      if (this.clicked) {
        this.monthListDay = []
        this.monthList = []
        this.selectDay = ''
        if (this.secondSelect === '') {
          this.$emit('update-date-end', day.clone())
        }
        this.secondSelect = ''
        this.$emit('update-date-start', day.clone(), 'update-date-end', '')
        this.firstSelect = day
        this.firstSelected = day.unix()
        this.clicked = !this.clicked
      } else {
        this.clicked = !this.clicked
        if (this.firstSelect <= day) {
          this.$emit('update-date-end', day.clone())
          this.secondSelect = day
          this.secondSelected = day.unix()
        }
      }
    },
    getDays() {
      const range1 = Moment.range(
        this.startMonth,
        this.startMonth.clone().endOf('month')
      )
      const list = Array.from(range1.by('days')).map((m) => m)
      this.monthList = list
      return list
    },
    getWeekStart() {
      return this.startMonth.weekday()
    },
    nextMonth() {
      let month = this.startMonth.add(1, 'M')
      let year = this.year
      if (month.format('M') > 11) {
        year = moment(year)
          .add(1, 'Y')
          .format('YYYY')
      }
      month = month.format('MMMM')
      this.month = month
      this.year = year
    },
    prevMonth() {
      let month = this.startMonth.subtract(1, 'M')
      let year = this.year
      if (month.format('M') > 11) {
        year = moment(year)
          .subtract(1, 'Y')
          .format('YYYY')
      }
      month = month.format('MMMM')
      this.month = month
      this.year = year
    },
    selectDateByDay(day) {
      this.selectDay = day
      const result = this.monthList.map((item) => {
        if (item.format('dddd') === day) {
          return item
        }
      })
      const res = result.filter((val) => val)
      this.monthListDay = res
    },
    showDatepicker() {
      this.isVisible = true
      setTimeout(
        () => document.addEventListener('click', this.hideDatePicker),
        0
      )
    },
    actionSubmit() {
      this.$emit('update-multiple', this.monthListDay)
      this.hideDatePicker()
    },
    cancel() {
      this.hideDatePicker()
    },
    hideDatePicker() {
      this.$emit('update-visible', false)
      document.removeEventListener('click', this.hideDatePicker)
    }
  }
}
</script>

<style>
.datepicker {
  z-index: 5;
  background-color: #fff;
  overflow: auto;
}
.datepicker__calendar {
  width: 315px;
  min-width: 315px;
  margin: auto;
}
.datepicker__options {
  width: 200px;
  border-left: solid 1px #ddd;
}
.datepicker__options button {
  text-align: left;
  padding: 10px 5px;
  display: flex;
  width: 100%;
}
.datepicker__options button:focus {
  outline: none;
}
.datepicker__options button.selected,
.datepicker__options button:hover {
  background-color: #63b3ed;
  color: #fff;
}

.datepicker__header {
  height: 100px;
  color: #fff;
  padding: 20px;
  text-align: left;
}
.datepicker__day {
  height: 41px;
  width: 41px;
  line-height: 41px;
  text-align: center;
  z-index: 10;
  position: relative;
  float: left;
  cursor: pointer;
  color: #000;
  transition: color 450ms cubic-bezier(0.075, 0.82, 0.165, 1);
}
.datepicker__day:hover {
  color: #fff;
}
.datepicker__day:hover .datepicker__day__effect {
  transform: scale(1);
}

.datepicker__day.selected {
  color: #fff;
}

.datepicker__day.selected .datepicker__day__effect {
  transform: scale(1);
  opacity: 0.6;
}

.datepicker__day__text {
  position: relative;
}
.datepicker__day__effect {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 36px;
  height: 36px;
  background-color: #63b3ed;
  transition: color 450ms cubic-bezier(0.075, 0.82, 0.165, 1);
  transform: scale(0);
}
.datepicker__days {
  position: relative;
  overflow: hidden;
  margin: 14px 14px 0;
}
.datepicker__year {
  position: relative;
  overflow: hidden;
  height: 16px;
  margin-bottom: 10px;
  line-height: 16px;
}
.datepicker__date {
  font-size: 32px;
  line-height: 32px;
  position: relative;
  text-transform: capitalize;
}
.datepicker__week {
  font-size: 12px;
  line-height: 12px;
  padding: 0 14px;
  height: 12px;
}
.datepicker__weekday {
  width: 41px;
  text-align: center;
  float: left;
}

.datepicker__controls {
  position: relative;
  height: 56px;
  line-height: 56px;
  text-align: center;
}
.datepicker__month {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  text-transform: capitalize;
}
.datepicker__prev {
  float: left;
  cursor: pointer;
  position: relative;
  height: 20px;
  width: 10px;
  left: 25px;
}
.datepicker__next {
  float: right;
  cursor: pointer;
  position: relative;
  height: 20px;
  width: 10px;
  right: 25px;
}
.datepicker-slide-transition {
  opacity: 1;
  transition: all 0.3s;
  transform: translateY(0);
}
.datepicker-slide-leave,
.datepicker-slide-enter {
  opacity: 0;
  transform: translateY(-15px);
}
</style>
