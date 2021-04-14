<template>
  <div>
    <h1>App</h1>
    <div class="approver__rows">
      <Row
        v-for="(record, key) in recordsByKey"
        v-bind="{ migrateFunction, schema, record, confirmFunction }"
        :key="key"
        :active="active === key"
        @selected="active = key"
      >
        <template v-slot:input="{ record }"
          ><slot name="input" v-bind="{ record }"
        /></template>
      </Row>
    </div>
  </div>
</template>

<script>
import { fromPairs } from "lodash-es";
import Row from "@/components/Row.vue";

export default {
  components: {
    Row,
  },
  computed: {
    recordsByKey() {
      return fromPairs(
        this.records.map((record) => [record[this.keyField], record])
      );
    },
  },
  data() {
    return {
      active: null,
    };
  },
  props: {
    keyField: {
      type: String,
      default: "id",
    },
    records: {
      type: Array,
      required: true,
    },
    schema: {
      type: Array,
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
};
</script>

<style lang="scss">
.approver {
  &__rows {
    display: grid;
    grid-gap: 20px;
  }
}
</style>
