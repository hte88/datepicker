<template>
  <div>
    <DatePickerInput
      :type-input="typeInput"
      :label="labelInput"
      :value="valueInput"
      @showDatepicker="showDatepicker"
    ></DatePickerInput>
    <DatePickerAgenda
      :date="date"
      :visible="isVisible"
      @valueInput="sendValueInput"
      @actionSubmit="actionSubmit"
      @actionCancel="actionCancel"
    ></DatePickerAgenda>
    {{ this.valueInput }}
  </div>
</template>
<script>
import DatePickerInput from '~/components/DatePickerInput'
import DatePickerAgenda from '~/components/DatePickerAgenda'

export default {
  components: {
    DatePickerInput,
    DatePickerAgenda
  },
  props: {
    typeInput: { type: String, default: null },
    labelInput: { type: String, default: null },
    date: { type: Object, default: null }
  },
  data() {
    return {
      isVisible: false,
      valueInput: null
    }
  },
  computed: {},
  methods: {
    /*
     ** Renvoi la date vers l'input
     */
    sendValueInput(data) {
      console.log(data)
      this.valueInput = data
    },
    /*
     ** Affiche le calendrier
     */
    showDatepicker() {
      this.isVisible = true
      setTimeout(
        () => document.addEventListener('click', this.hideDatePicker),
        1
      )
    },
    /*
     ** Valide le calendrier
     */
    actionSubmit() {
      this.hideDatePicker()
    },
    /*
     ** Ferme le calendrier
     */
    actionCancel() {
      this.hideDatePicker()
    },
    /*
     ** Cache le calendrier
     */
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
