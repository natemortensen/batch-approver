<template>
  <div class="row">
    <button class="row__arrow">&rarr;</button>

    <div class="row__input">
      <slot name="input" v-bind="{ record }" />
    </div>

    <div class="row__output" v-if="active">
      <FormulateForm
        :value="formData"
        :schema="schema"
        @input="(val) => $emit('input', val)"
        @submit="$emit('confirmed', record)"
      />
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    formData: {
      get() {
        return this.value;
      },
      set(val) {
        this.$emit("input", val);
      },
    },
  },
  props: {
    record: {
      type: Object,
      required: true,
    },
    value: {
      type: Object,
      required: true,
    },
    schema: {
      type: Array,
      required: true,
    },
    active: {
      type: Boolean,
      required: true,
    },
    completed: {
      type: Boolean,
      required: true,
    },
    migrateFunction: {
      type: Function,
      required: true,
    },
  },
  watch: {
    active() {
      this.$emit("input", this.migrateFunction(this.record));
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.row {
  display: flex;

  &__arrow {
    color: inherit;
    background: transparent;
    width: 30px;
    height: 30px;
    font-size: 30px;
    border: 2px solid currentColor;
    font-weight: bold;
  }

  &__input {
    flex-basis: 40%;
  }
}
</style>
