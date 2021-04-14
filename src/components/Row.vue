<template>
  <div class="row" @click="$emit('selected')" :class="{ active }">
    <div class="row__check" v-if="confirmed">&checkmark;</div>
    <button class="row__arrow" v-else>&rang;</button>

    <div class="row__input">
      <slot name="input" v-bind="{ record }" />
    </div>

    <div class="row__output" v-show="active" v-if="formData">
      <FormulateForm v-model="formData" :schema="schema" />
      <button type="button" v-if="!confirmed" @click="confirm">
        Confirm
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      confirmed: false,
      formData: null,
    };
  },
  methods: {
    async confirm() {
      await this.confirmFunction(this.formData);
      this.confirmed = true;
    },
  },
  props: {
    record: {
      type: Object,
      required: false,
    },
    schema: {
      type: Array,
      required: true,
    },
    active: {
      type: Boolean,
      required: true,
    },
    migrateFunction: {
      type: Function,
      required: true,
    },
    confirmFunction: {
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
  padding: 10px;
  display: grid;
  grid-gap: 20px;
  align-items: center;
  grid-template-columns: 30px 1fr 2fr;
  border-bottom: 1px solid #cbcaca;

  &__arrow,
  &__check {
    color: inherit;
    background: transparent;
    width: 30px;
    height: 30px;
    font-size: 30px;
    border: none;
    font-weight: bold;
    cursor: pointer;
    outline: none;
    transition: all 0.3s;
  }

  &__check {
    color: green;
  }

  .active &__arrow {
    transform: rotate(90deg);
  }

  &__input {
    flex-basis: 40%;
    cursor: pointer;
  }

  &__output {
    flex: 1;

    .formulate-form {
      width: 100%;
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
      grid-gap: 10px;
      align-items: center;
    }
  }
}
</style>
