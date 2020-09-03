<template>
  <div>
    <DatePickerInput
      :type-input="typeInput"
      :label-input="label"
      :value="valueInput"
      :value-start="valueInputStart"
      :value-end="valueInputEnd"
      @showDatepicker="showDatepicker"
      @monthSelect="sendValueSelect"
    ></DatePickerInput>
    <DatePickerAgenda
      :type-input="typeInput"
      :date="value"
      :visible="isVisible"
      @dateUpdate="$emit('input', $event)"
      @valueInput="sendValueInput"
      @update-multiple="multipleData"
      @update-date-start="updateStart"
      @update-date-end="updateEnd"
      @actionSubmit="actionSubmit"
      @actionCancel="actionCancel"
    ></DatePickerAgenda>
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
    value: {
      type: Object,
      default: () => {
        return moment()
      }
    },
    typeInput: { type: String, default: 'single' },
    label: { type: String, default: null }
  },
  data() {
    return {
      isVisible: false,
      valueInput: null,
      valueInputStart: null,
      valueInputEnd: null
    }
  },
  computed: {},
  methods: {
    sendValueSelect(data) {
      if (data) {
        this.$emit('input', data)
      }
    },
    sendValueInput(data) {
      this.valueInput = data.format('dddd DD MMMM YYYY')
      this.$emit('input', data)
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
