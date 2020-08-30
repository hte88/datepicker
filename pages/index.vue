<template>
  <div class="container">
    <div class="w-1/2 m-auto pt-20">
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold mb-10">
          Step 1 : Select
        </h1>
        <div class="w-full mb-3">
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
              <option
                v-for="month in months"
                :key="month"
                :value="month"
                class=""
              >
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
        <div v-if="openDatepicke" class="relative flew flex-wrap">
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
                  :value="dateFromSelect.format('DD MMMM YYYY')"
                  class="appearance-none text-gray-700 border rounded py-3 px-4 leading-tight focus:outline-none bg-white flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-r-none px-3 relative"
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
                  :value="dateEnd"
                  class="appearance-none text-gray-700 border rounded py-3 px-4 leading-tight focus:outline-none bg-white flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-r-none px-3 relative"
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
          <div class="w-full">
            <label
              class="block uppercase tracking-wide text-gray-700 text-xs font-bold mb-2"
            >
              Multiple
            </label>
            <div class="flex flex-wrap items-stretch w-full mb-4 relative">
              <input
                type="text"
                :value="multiple"
                class="appearance-none text-gray-700 border rounded py-3 px-4 leading-tight focus:outline-none bg-white flex-shrink flex-grow flex-auto leading-normal w-px flex-1 border h-10 border-grey-light rounded rounded-r-none px-3 relative"
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
          <date-picker
            :date="dateFromSelect"
            :visible="isVisible"
            @update-date-start="updateStart"
            @update-date-end="updateEnd"
            @update-multiple="multipleData"
            @update-visible="visible"
          ></date-picker>
        </div>
      </div>
      <!-- -->
    </div>
  </div>
</template>

<script>
import moment from 'moment'
import DatePicker from '~/components/DatePicker'
moment.locale('fr')
export default {
  name: 'App',
  components: { DatePicker },
  data() {
    return {
      date: moment(),
      dateFromSelect: moment('2000-01-01', 'DD-MM-YYYY'),
      dateEnd: '',
      months: moment.months(),
      isVisible: false,
      openDatepicke: false,
      multiple: []
    }
  },
  computed: {},
  methods: {
    setMonth(value) {
      const newValue = moment('01' + value + '2020', 'DD MMMM YYYY')
      this.dateFromSelect = newValue
      this.openDatepicke = true
      this.isVisible = true
    },
    showDatepicker() {
      this.isVisible = true
    },
    updateStart(updateDate) {
      if (updateDate) {
        this.dateFromSelect = updateDate
      }
    },
    updateEnd(updateDate) {
      if (updateDate) {
        this.dateEnd = updateDate.format('DD MMMM YYYY')
      }
    },
    visible(data) {
      this.isVisible = data
    },
    multipleData(data) {
      const result = data.map((item) => {
        return item.format('dddd DD MMMM')
      })
      this.multiple = result
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
}
</style>
