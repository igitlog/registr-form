<template>
  <div>
    <el-form-item :label="label" :prop="prop" :rules="rules">
      <el-input v-model="internalValue" :class="{inputClass: true}" :show-password="showPassword" :type="type" :placeholder="placeholder" @input="handleInput"></el-input>
    </el-form-item>
  </div>
</template>

<script>
export default {
  name: "InputEl",
  props: {
    label: String,
    prop: String,
    rules: Array,
    showPassword: Boolean,
    modelValue: [String, Number],
    type: {
      type: String,
      default: 'text'
    },
    placeholder: String
  },
  data() {
    return {
      internalValue: this.modelValue
    };
  },
  methods: {
    handleInput(value) {
      this.internalValue = value;
      this.$emit('update:modelValue', value, this.prop);
    }
  },
  mounted() {
    console.log(this.rules)
  },
};
</script>

<style lang="scss">
  .inputClass input {
    border: 1px solid #E6E6EB;
    font-family: Montserrat;
    font-size: 14px;
    font-weight: 400;
    line-height: 19px;
    letter-spacing: -0.0015em;
    text-align: left;
    border-radius: 11px;
  }
</style>