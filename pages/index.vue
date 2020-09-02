<template>
  <div class="container mx-auto px-4 min-h-screen">
    <div class="w-full sm:w-4/5 md:w-4/5 lg:w-1/2 m-auto py-20 px-3">
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold">
          Step 1 : Select
        </h1>
        <h2 class=" font-semibold text-gray-800 mb-10">
          Select a month then a day
        </h2>
        <div class="w-full mb-3">
          <date-picker
            v-model="date_1"
            type-input="select"
            label="By Month"
          ></date-picker>
          <p>Date selected : {{ date_1.format('DD MMMM YYYY') }}</p>
        </div>
      </div>

      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold">
          Step 2 : Single
        </h1>
        <h2 class=" font-semibold text-gray-800 mb-10">
          Select a single day
        </h2>
        <div class="w-full mb-3">
          <date-picker
            v-model="date_2"
            type-input="single"
            label="Date"
          ></date-picker>
        </div>
      </div>
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold">
          Step 3 : Start and End
        </h1>
        <h2 class=" font-semibold text-gray-800 mb-10">
          Select a period
        </h2>
        <div class="w-full mb-3">
          <date-picker v-model="date_3" type-input="startEnd"></date-picker>
        </div>
      </div>
      <div class="w-full border-b pb-10 mb-10">
        <h1 class="text-left text-xl uppercase font-semibold">
          Step 4 : Weekday
        </h1>
        <h2 class=" font-semibold text-gray-800 mb-10">
          Select a weekday by example : click on Monday or Tuesday, ...
        </h2>
        <div class="w-full mb-3">
          <date-picker
            v-model="date_4"
            type-input="multiple"
            label="Multiple"
          ></date-picker>
        </div>
      </div>
      <!-- -->
      <div class="w-full">
        <h1 class="uppercase text-4xl border-b mb-5">Usage</h1>
        <textarea
          class="p-3 w-full h-16 border focus:outline-none bg-white overflow-hidden"
        >
<date-picker v-model="date"></date-picker>
</textarea
        >
      </div>
      <div class="w-full mt-10">
        <h1 class="uppercase text-4xl border-b mb-5">Available props</h1>
        <table class="table-fixed">
          <thead>
            <tr class="bg-white">
              <th class="w-2/6">Prop</th>
              <th class="w-1/6">Type</th>
              <th class="w-1/6">Default</th>
              <th class="w-2/6">Description</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>date</td>
              <td>String</td>
              <td></td>
              <td>Date value of the datepicker</td>
            </tr>
            <tr>
              <td>label</td>
              <td>String</td>
              <td></td>
              <td>Label for the input</td>
            </tr>
            <tr>
              <td>type-input</td>
              <td>String</td>
              <td>single</td>
              <td>
                Option for a calendar (select | single | startEnd | multiple)
              </td>
            </tr>
          </tbody>
        </table>
      </div>
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
      date_1: moment(),
      date_2: moment(),
      date_3: moment(),
      date_4: moment()
    }
  },
  computed: {},
  methods: {}
}
</script>

<style>
table td,
table th {
  border: solid 1px #ddd;
  padding: 2rem 1rem;
}

table tbody tr:nth-child(even) {
  background-color: #fff;
}
</style>
