<template>
  <div class="container">
    <div class="w-full sm:w-4/5 md:w-4/5 lg:w-1/2 m-auto pt-20 px-3">
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold mb-10">
          Step 1 : Select
        </h1>
        <div class="w-full mb-3">
          <date-picker
            :date="dateFromSelect"
            :type-input="'select'"
            :label="'By Month'"
            @update-date-start="updateStart"
            @update-date-end="updateEnd"
            @update-multiple="multipleData"
          ></date-picker>
        </div>
      </div>
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold mb-10">
          Step 2 : Single Input
        </h1>
        <div class="w-full mb-3">
          <date-picker
            :date="dateFromSelect"
            :type-input="'single'"
            :label="'Date'"
            @update-date-start="updateStart"
            @update-date-end="updateEnd"
            @update-multiple="multipleData"
          ></date-picker>
        </div>
      </div>
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold mb-10">
          Step 3 : Start and End
        </h1>
        <div class="w-full mb-3">
          <date-picker
            :date="dateFromSelect"
            :type-input="'startEnd'"
            :label="null"
            @update-date-start="updateStart"
            @update-date-end="updateEnd"
            @update-multiple="multipleData"
          ></date-picker>
        </div>
      </div>
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold mb-10">
          Step 4 : Custom By Weekday
        </h1>
        <div class="w-full mb-3">
          <date-picker
            :date="dateFromSelect"
            :type-input="'multiple'"
            :label="'Multiple'"
            @update-date-start="updateStart"
            @update-date-end="updateEnd"
            @update-multiple="multipleData"
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
      multiple: []
    }
  },
  computed: {},
  methods: {
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
    /*
    visible(data) {
      this.isVisible = data
    },
    */
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
