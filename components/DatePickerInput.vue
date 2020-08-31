<template>
  <div>
    <!-- Type Input Only -->
    <div v-if="typeInput === 'select'">
      <label
        class="flex uppercase tracking-wide text-gray-700 text-xs font-bold"
        >By Month</label
      >
      <div class="inline-block relative w-full">
        <select
          class="capitalize block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded leading-tight focus:outline-none"
          value="Janvier"
          @input="setMonth($event.target.value)"
        >
          <option selected value="null">Choose month</option>
          <option v-for="month in months" :key="month" :value="month" class="">
            {{ month }}
          </option>
        </select>
        <div
          class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700"
        >
          <svg
            class="fill-current h-4 w-4"
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20"
          >
            <path
              d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"
            />
          </svg>
        </div>
      </div>
    </div>
    <!-- Type Input Only -->
    <div v-if="typeInput === 'single'">
      <label
        class="flex uppercase tracking-wide text-gray-700 text-xs font-bold"
        >Date</label
      >
      <div class="flex flex-wrap items-stretch w-full mb-4 relative">
        <input
          type="text"
          :value="valueInput"
          class="appearance-none text-gray-700 py-3 px-4 focus:outline-none bg-white flex-shrink flex-grow leading-normal w-px border h-10 border-grey-light rounded rounded-r-none relative"
          placeholder="01-01-2000"
          @click="showDatepicker"
        />
        <div class="flex -mr-px">
          <span
            class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
            @click="showDatepicker"
          >
            <img
              :src="require('../assets/img/calendar-alt-regular.svg')"
              class="w-6 h-6"
              alt="Calendar"
            />
          </span>
        </div>
      </div>
    </div>
    <!-- Type Input Start & End -->
    <div v-if="typeInput === 'startEnd'">
      <div class="flex justify-between">
        <div class="w-1/2 pr-1">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
          >
            Start
          </label>
          <div class="flex flex-wrap items-stretch w-full mb-4 relative">
            <input
              type="text"
              :value="valueInput"
              class="appearance-none text-gray-700 py-3 px-4 focus:outline-none bg-white flex-shrink flex-grow leading-normal w-px border h-10 border-grey-light rounded rounded-r-none relative"
              @click="showDatepicker"
            />
            <div class="flex -mr-px">
              <span
                class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
                @click="showDatepicker"
              >
                <img
                  :src="require('../assets/img/calendar-alt-regular.svg')"
                  class="w-6 h-6"
                  alt="Calendar"
                />
              </span>
            </div>
          </div>
        </div>
        <div class="w-1/2 pl-1">
          <label
            class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
          >
            end
          </label>
          <div class="flex flex-wrap items-stretch w-full mb-4 relative">
            <input
              type="text"
              :value="valueInput"
              class="appearance-none text-gray-700  py-3 px-4 focus:outline-none bg-white flex-shrink flex-grow leading-normal w-px border h-10 border-grey-light rounded rounded-r-none relative"
              @click="showDatepicker"
            />
            <div class="flex -mr-px">
              <span
                class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
                @click="showDatepicker"
              >
                <img
                  :src="require('../assets/img/calendar-alt-regular.svg')"
                  class="w-6 h-6"
                  alt="Calendar"
                />
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- Type Input Multiple -->
    <div v-if="typeInput === 'multiple'">
      <label
        class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
      >
        Multiple
      </label>
      <div class="flex flex-wrap items-stretch w-full mb-4 relative">
        <input
          type="text"
          :value="valueInput"
          class="appearance-none text-gray-700 border rounded py-3 px-4 focus:outline-none bg-white flex-shrink flex-grow leading-normal w-px h-10 border-grey-light rounded-r-none relative"
          @click="showDatepicker"
        />
        <div class="flex -mr-px">
          <span
            class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
            @click="showDatepicker"
          >
            <img
              :src="require('../assets/img/calendar-alt-regular.svg')"
              class="w-6 h-6"
              alt="Calendar"
            />
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment'
moment.locale('fr')
export default {
  props: {
    labelInput: { type: String, default: null },
    typeInput: { type: String, default: null },
    valueInput: { type: Object, default: null },
    isVisible: { type: Boolean, default: false }
  },
  data() {
    return {
      dateFromSelect: moment('2000-01-01', 'DD-MM-YYYY'),
      openDatepicke: false,
      months: moment.months()
    }
  },
  methods: {
    init() {},
    setMonth(value) {
      const newValue = moment('01' + value + '2020', 'DD MMMM YYYY')
      this.dateFromSelect = newValue
      this.openDatepicke = true
      this.showDatepicker()
    },
    showDatepicker() {
      this.$emit('showDatepicker', true)
    }
  }
}
</script>

<style></style>
