<template lang="pug">
label.zi-radio-container(:class="classes")
  span.zi-radio(:class="classes")
  input.zi-radio-original(
    type="radio"
    name="-1"
    :value="label"
    :disabled="isDisabled"
    v-model="model"
  )
  span.zi-radio-label
    slot
</template>

<script>
export default {
  name: 'zi-radio',

  data: () => ({
    _model: false,
  }),

  props: {
    label: [String, Number],
    value: [String, Boolean, Number],
    disabled: {
      type: Boolean,
      defaultValue: false,
    },
  },

  computed: {
    model: {
      get() {
        if (this.inGroup) return this._group.model
        if (this.value !== undefined) return this.value
        return this._model
      },
      set(val) {
        if (this.inGroup) this._group.setLabelStatus(val)
        this.$emit('input', val)
      },
    },

    isDisabled() {
      if (this.inGroup) return this._group.disabled || this.disabled
      return this.disabled
    },

    classes() {
      let str = ''
      this.isDisabled && (str += ' disabled')
      this.label === this.model && (str += ' checked')
      return str
    },

    inGroup() {
      let parent = this.$parent
      while (parent) {
        if (parent.$options.name === 'zi-radio-group') {
          this._group = parent
          return true
        }
        parent = parent.$parent
      }
      return false
    },
  },
}
</script>
