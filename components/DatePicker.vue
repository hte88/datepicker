<template>
  <div>
    <DatePickerInput
      :type-input="typeInput"
      :label-input="label"
      :value="valueInput"
      :value-start="valueInputStart"
      :value-end="valueInputEnd"
      @showDatepicker="showDatepicker"
      @valueSelect="sendValueSelect"
    ></DatePickerInput>
    <DatePickerAgenda
      :type-input="typeInput"
      :date="date"
      :visible="isVisible"
      :month-select="valueSelect"
      @valueInput="sendValueInput"
      @update-multiple="multipleData"
      @update-date-start="updateStart"
      @update-date-end="updateEnd"
      @actionSubmit="actionSubmit"
      @actionCancel="actionCancel"
    ></DatePickerAgenda>
    <input
      v-if="typeInput === 'select' && valueInput !== null"
      type="text"
      class="cursor-not-allowed mt-2 appearance-none text-gray-700 py-3 px-4 focus:outline-none bg-white leading-normal w-full border h-10 border-grey-light rounded relative"
      :value="valueInput"
      disabled
    />
  </div>
</template>
<script>
import moment from 'moment'
import DatePickerInput from '~/components/DatePickerInput'
import DatePickerAgenda from '~/components/DatePickerAgenda'
moment.locale('fr')
export default {
  components: {
    DatePickerInput,
    DatePickerAgenda
  },
  props: {
    typeInput: { type: String, default: null },
    label: { type: String, default: null },
    date: { type: Object, default: null },
    format: { type: String, default: 'DD MMMM YYYY' }
  },
  data() {
    return {
      isVisible: false,
      valueInput: null,
      valueInputStart: null,
      valueInputEnd: null,
      valueSelect: null
    }
  },
  computed: {},
  methods: {
    formattedDate(data) {
      return data.format(this.format)
    },
    sendValueSelect(data) {
      if (data) {
        this.valueSelect = data
      }
    },
    sendValueInput(data) {
      this.valueInput = data.format('dddd DD MMMM YYYY')
    },
    multipleData(data) {
      const result = data.map((item) => {
        return item.format('dddd DD MMMM')
      })
      this.valueInput = result.toString()
    },
    updateStart(data) {
      if (data) {
        this.valueInputStart = data.format('DD MMMM YYYY')
      }
    },
    updateEnd(data) {
      if (data) {
        this.valueInputEnd = data.format('DD MMMM YYYY')
      }
    },
    showDatepicker() {
      this.isVisible = true
      setTimeout(
        () => document.addEventListener('click', this.hideDatePicker),
        1
      )
    },
    actionSubmit() {
      this.hideDatePicker()
    },
    actionCancel() {
      this.hideDatePicker()
    },
    hideDatePicker() {
      this.isVisible = false
      setTimeout(
        () => document.removeEventListener('click', this.hideDatePicker),
        0
      )
    }
  }
}
</script>
