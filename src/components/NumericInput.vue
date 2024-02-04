<template>
  <span class="numeric-input">
    <input
      type="number"
      :id="fieldId"
      :min="_min"
      :max="_max"
      :step="_step"
      :pattern="_pattern"
      :placeholder="_placeholder"
      :value="val"
      v-on:change="inputChange($event)"
      v-on:blur="inputBlur($event)" />
    <button
      type="button"
      value="+"
      v-on:click="btnClick($event)">+</button>
    <button
      type="button"
      value="-"
      v-on:click="btnClick($event)">-</button>
  </span>

</template>

<script setup>
import { ref, computed, onBeforeMount } from 'vue';

// --------------------------------------------------
// START: Data types

//  END:  Data types
// --------------------------------------------------
// START: Component config

const emit = defineEmits('blur', 'change', 'invalid');

//  END:  Component config
// --------------------------------------------------
// START: Properties/attributes

const props = defineProps({
  fieldId: { type: String, required: true },
  labeledBy: { type: String, required: true },
  describedBy: { type: String, required: false, default: '' },
  value: { required: false, default: '' },
  min: { type: Number, required: false, default: null },
  max: { type: Number, required: false, default: null },
  pattern: { type: String, required: false, default: '' },
  placeholder: { type: String, required: false, default: '' },
  step: { type: Number, required: false, default: 1 },
});

//  END:  Properties/attributes
// --------------------------------------------------
// START: Local state

const val = ref('');
const minNum = ref(null);
const maxNum = ref(null);
const inputStep = ref(null);

//  END:  Local state
// --------------------------------------------------
// START: Computed properties

const _min = computed(() => { // eslint-disable-line arrow-body-style
  return (minNum.value !== null)
    ? minNum.value
    : undefined;
});

const _max = computed(() => { // eslint-disable-line arrow-body-style
  return (maxNum.value !== null)
    ? maxNum.value
    : undefined;
});

const _step = computed(() => { // eslint-disable-line arrow-body-style
  return (inputStep.value !== null)
    ? inputStep.value
    : undefined;
});

const _pattern = computed(() => { // eslint-disable-line arrow-body-style
  return (typeof props.pattern === 'string' && props.pattern.trim() !== '')
    ? props.pattern
    : undefined;
});

const _placeholder = computed(() => { // eslint-disable-line arrow-body-style
  return (typeof props.placeholder === 'string' && props.placeholder.trim() !== '')
    ? props.placeholder
    : undefined;
});

//  END:  Computed properties
// --------------------------------------------------
// START: Local methods

const btnClick = (event) => {
  console.group('btnClick()');
  console.log('event.target.value:', event.target.value);
  const tmp = (event.target.value === '+')
    ? inputStep.value
    : inputStep.value * -1;
  console.log('tmp:', tmp);
  console.log('val.value:', val.value);
  console.log('typeof val.value:', typeof val.value);
  console.log('typeof val.value === "string":', typeof val.value === 'string');


  const _v = (typeof val.value === 'string')
    ? (val.value === '')
      ? 0
      : parseInt(val.value, 10)
    : val.value;
  console.log('_v:', _v);

  const output = _v + tmp;
  console.log('output:', output);

  if ((minNum.value === null || output >= minNum.value) && (maxNum.value === null || output <= maxNum.value)) {
    val.value = output;
  }
  console.log('val.value:', val.value);
  console.groupEnd();
}

const inputChange = (event) => {
  const { value } = event.target;

  if (value < minNum.value) {
    val.value = minNum.value;
    event.target.value = val.value; // eslint-disable-line no-var-reassign
  } else if (value > maxNum.value) {
    val.value = maxNum.value;
    event.target.value = val.value; // eslint-disable-line no-var-reassign
  } else {
    val.value = value;
  }

  emit('invalid', event.target.validity.valid);

  emit('change', event);
}


const inputBlur = (event) => {
  emit('blur', event);
};

const sanitiseNum = (val) => {
  const minT = typeof val
  if (minT === 'number') {
    return val;
  }

  if (minT === 'string' && val.trim() !== '') {
    return parseFloat(val);
  }

  return null;
}

//  END:  Local methods
// --------------------------------------------------
// START: Lifecycle methods

onBeforeMount(() => {
  minNum.value = sanitiseNum(props.min);
  maxNum.value = sanitiseNum(props.max);
  inputStep.value = sanitiseNum(props.step)
});

//  END:  Lifecycle methods
// --------------------------------------------------
</script>


<style scoped>
.read-the-docs {
  color: #888;
}
</style>
