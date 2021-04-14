<template>
  <div class="row">
    <button class="row__arrow" @click="$emit('selected')">&rang;</button>

    <div class="row__input">
      <slot name="input" v-bind="{ record }" />
    </div>

    <div class="row__output" v-show="active">
      <FormulateForm
        v-if="formData"
        v-model="formData"
        :schema="schema"
        @submit="confirmed = true"
      />
    </div>
  </div>
</template>

<script>
import "@braid/vue-formulate/themes/snow/snow.scss";

export default {
  data() {
    return {
      confirmed: false,
      formData: null,
    };
  },
  props: {
    record: {
      type: Object,
      required: false,
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
      if (!this.formData) {
        this.formData = this.migrateFunction(this.record);
      }
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

  &__output {
    flex: 1;

    .formulate-form {
      width: 100%;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    }
  }
}
</style>
