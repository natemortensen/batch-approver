<template>
  <div>
    <h1>App</h1>
    <Row
      v-for="record in records"
      v-bind="{ migrateFunction, schema, record }"
      :key="record[keyField]"
      :active="active === record[keyField]"
      @selected="active = record[keyField]"
    >
      <template v-slot:input="{ record }"
        ><slot name="input" v-bind="{ record }"
      /></template>
    </Row>
  </div>
</template>

<script>
import { fromPairs } from "lodash-es";
import Row from "@/components/Row.vue";

export default {
  components: {
    Row,
  },
  data() {
    return {
      active: null,
      completed: fromPairs(this.records.map((r) => [r[this.keyField], false])),
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
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
