<template>
  <input
    :step="step"
    :min="min"
    :max="max"
    :disabled="disabled"
    :value="value"
    :base="base"
    :precision="precision"
    type="range"
    @input="emit($event.target.value)"
    @touchmove="touchmove"
    @touchstart="touchmove"
  >
</template>

<script>
export default {
  name: 'TouchRange',
  props: {
    value: { type: Number, default: 0 },
    min: { type: Number, default: 0 },
    max: { type: Number, default: 1 },
    base: { type: Number, default: 10 },
    precision: { type: Number, default: 0 },
    step: { type: [Number, String], default: 'any' },
    disabled: { type: Boolean, default: false }
  },
  computed: {
    range() {
      return this.max - this.min
    }
  },
  methods: {
    emit(val) {
      this.$emit('input', Math.round(val * Math.pow(this.base, this.precision)) / Math.pow(this.base, this.precision))
    },
    touchmove(e) {
      e.preventDefault()
      const cw = e.target.clientWidth
      const rate = Math.max(0, Math.min(cw, e.touches[0].pageX - e.target.offsetLeft)) / cw
      this.emit(rate * this.range + this.min)
    }
  }
}
</script>
