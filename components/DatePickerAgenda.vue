<template>
  <div v-if="visible">
    <div
      class="relative mt-3 bg-white overflow-auto z-10 border shadow rounded-lg flex"
      @click.stop
    >
      <div class="datepicker_options border-l bg-blue-400">
        <div class="h-20 text-white text-left p-4 bg-blue-400">
          <div class="relative overflow-hidden mb-2 leading-3 h-3">
            {{ year }}
          </div>
          <div class="relative capitalize text-3xl leading-8">
            {{ date.format('dddd DD MMMM') }}
          </div>
        </div>
      </div>
      <div class="datepicker_calendar m-auto">
        <div
          class="h-10 leading-10 my-2 flex items-center justify-between text-center relative"
        >
          <div class="capitalize inset-0 absolute text-xl">
            {{ month }}
          </div>
          <button
            class="float-left ml-5 cursor-pointer w-3 h-4 relative focus:outline-none"
            @click="prevMonth()"
          >
            <font-awesome-icon
              :icon="['fas', 'chevron-left']"
              class="absolute left-0 top-0 fa-lg hover:text-blue-500"
            />
          </button>
          <button
            class="float-right mr-5 cursor-pointer w-3 h-4 relative focus:outline-none"
            @click="nextMonth()"
          >
            <font-awesome-icon
              :icon="['fas', 'chevron-right']"
              class="absolute right-0 top-0 fa-lg hover:text-blue-500"
            />
          </button>
        </div>
        <div v-if="typeInput === 'multiple'" class="h-3 leading-3 p-3 text-xs">
          <div
            v-for="day in days"
            :key="day.index"
            class="datepicker_weekday text-center float-left cursor-pointer hover:text-blue-500"
            @click="selectDateByDay(day.long)"
          >
            {{ day.short }}
          </div>
        </div>
        <div v-else class="h-3 leading-3 p-3 text-xs">
          <div
            v-for="day in days"
            :key="day.index"
            class="datepicker_weekday text-center float-left"
          >
            {{ day.short }}
          </div>
        </div>
        <div class="relative overflow-hidden mx-3 mt-3">
          <div
            class="datepicker_day"
            :style="{ width: getWeekStart() * 41 + 'px' }"
          ></div>
          <div
            v-for="day in getDays()"
            :key="day.i"
            class="datepicker_day"
            :class="{ selected: isSelected(day) }"
          >
            <div
              v-if="
                firstSelect > day &&
                  secondSelect === null &&
                  typeInput === 'startEnd'
              "
            >
              <span class="relative text-gray-300">{{ day.format('DD') }}</span>
            </div>
            <div v-else @click="selectDate(day)">
              <span class="datepicker_day_effect"></span>
              <span class="relative">{{ day.format('DD') }}</span>
            </div>
          </div>
        </div>
        <div class="clear-both"></div>
        <div class="datepicker__actions justify-end flex p-5">
          <button
            class="px-4 py-2 hover:text-blue-500 hover:bg-gray-200 font-semibold"
            @click="actionCancel"
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
moment.locale('fr')

export default {
  components: {},
  props: {
    date: { type: Object, default: null },
    visible: { type: Boolean, default: false },
    typeInput: { type: String, default: null },
    monthSelect: { type: Object, default: null }
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
      selectDay: null,
      clicked: true,
      firstSelect: null,
      firstSelected: null,
      secondSelect: null,
      secondSelected: null
    }
  },
  computed: {},
  methods: {
    isSelected(day) {
      if (this.selectDay === null) {
        if (this.firstSelected !== null) {
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
    initValCalendar() {
      this.monthListDay = []
      this.monthList = []
      this.selectDay = null
      this.secondSelect = null
    },
    selectOneDay(day) {
      this.$emit('valueInput', day.clone())
      this.firstSelect = day
      this.firstSelected = day.unix()
    },
    selectTwoDays(day) {
      if (this.clicked === true) {
        this.monthListDay = []
        this.monthList = []
        this.selectDay = null
        if (this.secondSelect === null) {
          this.$emit('update-date-end', day.clone())
        }
        this.secondSelect = null
        this.$emit('update-date-start', day.clone(), 'update-date-end', null)
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
    selectDate(day) {
      switch (this.typeInput) {
        case 'select':
        case 'single':
          this.selectOneDay(day)
          break
        case 'startEnd':
          this.selectTwoDays(day)
          break
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
      this.$emit('update-multiple', res)
    },
    actionSubmit() {
      this.$emit('actionSubmit', false)
    },
    actionCancel() {
      this.$emit('actionCancel', false)
    }
  }
}
</script>

<style>
.datepicker_calendar {
  width: 315px;
  min-width: 315px;
}
.datepicker_options {
  width: 200px;
}
.datepicker_options button {
  text-align: left;
  padding: 10px 5px;
  display: flex;
  width: 100%;
}
.datepicker_options button:focus {
  outline: none;
}
.datepicker__options button.selected,
.datepicker__options button:hover {
  background-color: #63b3ed;
  color: #fff;
}

.datepicker_day {
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
.datepicker_day:hover {
  color: #fff;
}
.datepicker_day:hover .datepicker_day_effect {
  transform: scale(1);
}

.datepicker_day.selected {
  color: #fff;
}

.datepicker_day.selected .datepicker_day_effect {
  transform: scale(1);
  opacity: 0.6;
}
.datepicker_day_effect {
  position: absolute;
  top: 2px;
  left: 2px;
  width: 36px;
  height: 36px;
  background-color: #63b3ed;
  transition: color 450ms cubic-bezier(0.075, 0.82, 0.165, 1);
  transform: scale(0);
}
.datepicker_weekday {
  width: 41px;
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
