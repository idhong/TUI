<template>
  <label class="t-radio"  :class="{
        'is-checked': val == (isGroup ? this.$parent.value : this.value),
        'is-disabled': disabled,
        'is-outbox': outbox,
      }">
    <span class="t-radio__label" v-if="labelLeft && !!label">{{ label }}</span>
    <span class="t-radio__inner">
        <span class="t-radio__circle"></span>
    </span>
    <span class="t-radio__label" v-if="!labelLeft && !!label">{{ label }}</span>
    <input type="radio" :disabled="disabled" :value="val" v-model="model">
    <br>
  </label>
</template>

<script>
import Emitter from '../../mixins/emitter'

export default {
  name: 't-radio',

  mixins: [Emitter],

  data () {
    return {
      isGroup: false,
      RadioGroup: null
    }
  },
  props: {
    label: String,
    labelLeft: Boolean,
    disabled: Boolean,
    outbox: Boolean,
    val: {
      type: String,
      required: true
    },
    value: {}
  },
  mounted () {
    this._isGroup()
  },
  methods: {
    _isGroup () {
      if (this.$parent.$options.name === 't-radio-group') {
        this.isGroup = true
        this.RadioGroup = this.$parent
        if (this.val === undefined) {
          throw new Error('Please giving radio [' + this.label + ']  a value like :val="value"')
        }
      }
    }
  },
  computed: {
    model: {
      get () {
        return this.isGroup ? this.RadioGroup.value : this.value
      },

      set (val) {
        this.isGroup ? this.dispatch('t-radio-group', 'input', val) : this.$emit('input', val)
      }
    }
  }
}
</script>

<style scoped>

</style>
