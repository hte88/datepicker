<template>
  <div>
    <!-- Type Input Only -->
    <div v-if="typeInput === 'select'">
      <label
        v-if="labelInput"
        class="flex uppercase tracking-wide text-gray-700 text-xs font-bold"
        >{{ labelInput }}</label
      >
      <div class="inline-block relative w-full">
        <select
          class="capitalize block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded leading-tight focus:outline-none"
          value="Janvier"
          @input="setMonth($event.target.value)"
        >
          <option selected value="null" disabled>Choose month</option>
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
        v-if="labelInput"
        class="flex uppercase tracking-wide text-gray-700 text-xs font-bold"
        >{{ labelInput }}</label
      >
      <div class="flex flex-wrap items-stretch w-full mb-4 relative">
        <input
          type="text"
          :value="value"
          disabled
          class="cursor-not-allowed appearance-none text-gray-700 py-3 px-4 focus:outline-none bg-gray-200 flex-shrink flex-grow leading-normal w-px border h-10 border-grey-light rounded rounded-r-none relative"
          @click="showDatepicker"
        />
        <div class="flex -mr-px">
          <span
            class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
            @click="showDatepicker"
          >
            <font-awesome-icon
              :icon="['far', 'calendar-alt']"
              class="fa-lg hover:text-blue-500 cursor-pointer"
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
              :value="valueStart"
              disabled
              class="cursor-not-allowed appearance-none text-gray-700 py-3 px-4 focus:outline-none bg-gray-200 flex-shrink flex-grow leading-normal w-px border h-10 border-grey-light rounded rounded-r-none relative"
              @click="showDatepicker"
            />
            <div class="flex -mr-px">
              <span
                class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
                @click="showDatepicker"
              >
                <font-awesome-icon
                  :icon="['far', 'calendar-alt']"
                  class="fa-lg hover:text-blue-500 cursor-pointer"
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
              :value="valueEnd"
              disabled
              class="cursor-not-allowed appearance-none text-gray-700  py-3 px-4 focus:outline-none bg-gray-200 flex-shrink flex-grow leading-normal w-px border h-10 border-grey-light rounded rounded-r-none relative"
              @click="showDatepicker"
            />
            <div class="flex -mr-px">
              <span
                class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
                @click="showDatepicker"
              >
                <font-awesome-icon
                  :icon="['far', 'calendar-alt']"
                  class="fa-lg hover:text-blue-500 cursor-pointer"
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
        v-if="labelInput"
        class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
      >
        {{ labelInput }}
      </label>
      <div class="flex flex-wrap items-stretch w-full mb-4 relative">
        <input
          type="text"
          :value="value"
          disabled
          class="cursor-not-allowed appearance-none text-gray-700 border rounded py-3 px-4 focus:outline-none bg-gray-200 flex-shrink flex-grow leading-normal w-px h-10 border-grey-light rounded-r-none relative"
          @click="showDatepicker"
        />
        <div class="flex -mr-px">
          <span
            class="flex items-center leading-normal bg-grey-lighter rounded rounded-l-none border border-l-0 border-grey-light px-3 whitespace-no-wrap text-grey-dark text-sm"
            @click="showDatepicker"
          >
            <font-awesome-icon
              :icon="['far', 'calendar-alt']"
              class="fa-lg hover:text-blue-500 cursor-pointer"
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
    typeInput: { type: String, default: 'single' },
    value: { type: String, default: null },
    valueStart: { type: String, default: null },
    valueEnd: { type: String, default: null },
    isVisible: { type: Boolean, default: false }
  },
  data() {
    return {
      dateFromSelect: moment('01 01 2020', 'DD MM YYYY'),
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
      this.$emit('valueSelect', newValue)
      this.showDatepicker()
    },
    showDatepicker() {
      this.$emit('showDatepicker', true)
    }
  }
}
</script>

<style></style>
