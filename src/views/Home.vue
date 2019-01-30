<template>
  <div class="grid-x grid-margin-x grid-padding-x">
    <div class="cell medium-4">
      <h3>Masking</h3>
      <input
        type="text"
        v-model="value"
        ref="focusableData"
        @click="focusInputBox"
        @focus="focusInputBox"
        v-mask="mask"
        @input="handleInputBox"
      />
      <p>{{ maskedValue }}</p>
    </div>
  </div>
</template>

<script>
import Inputmask from 'inputmask';

/* eslint-disable no-unused-vars */
Inputmask.extendDefinitions({
  x: {
    validator(chrs, buffer, pos, strict, opts) {
      const value = new RegExp('([0-9]|0[0-9]|1[0-9]|2[0-3])');
      return value.test(buffer[pos - 3] + buffer[pos - 2] + buffer[pos - 1] + chrs);
    },
    definitionSymbol: 'i',
  },
  5: {
    validator: '[0-5]',
    definitionSymbol: 'i',
  },
});

// Taken from https://github.com/scleriot/vue-inputmask/blob/master/src/vue-inputmask.js
const Mask = {
  bind: (el, binding) => {
    Inputmask(binding.value).mask(el);
  },
};

export default {
  data() {
    return {
      value: '00:00',
      mask: 'xx:59',
    };
  },
  directives: {
    mask: Mask,
  },
  computed: {
    maskedValue() {
      if (this.value && this.mask) return Inputmask.format(this.value, this.mask);
      return this.value;
    },
  },
  methods: {
    focusInputBox() {
      const input = this.$refs.focusableData;
      input.setSelectionRange(0, input.value.length);
    },
    handleInputBox(event) {
      this.$emit('input', event.target.inputmask.unmaskedvalue());
    },
  },
};
</script>
