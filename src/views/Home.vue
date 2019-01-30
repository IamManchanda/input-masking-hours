<template>
  <div class="grid-x grid-margin-x grid-padding-x">
    <div class="cell medium-4">
      <h3>Masking (HH:MM)</h3>
      <p><small>Only accepts values from <code>00:00</code> to <code>23:59</code></small></p>
      <input
        type="text"
        v-model="value"
        ref="focusableData"
        @click="focusInputBox"
        @focus="focusInputBox"
        v-mask="mask"
        @input="handleInputBox"
      />
    </div>
  </div>
</template>

<script>
import Inputmask from 'inputmask';

Inputmask.extendDefinitions({
  x: {
    validator(chrs, buffer, pos) {
      const value = new RegExp('([0-1][0-9])|(2[0-3])');
      const str = buffer.buffer[pos - 1] + chrs;
      return value.test(str);
    },
    definitionSymbol: 'i',
  },
  2: {
    validator: '[0-2]',
    definitionSymbol: 'i',
  },
  5: {
    validator: '[0-5]',
    definitionSymbol: 'i',
  },
});

Inputmask.extendAliases({
  numeric: {
    mask: 'x',
    greedy: false,
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
      mask: '2x:59',
    };
  },
  directives: {
    mask: Mask,
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
