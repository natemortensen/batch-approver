<template>
  <div>
    <h1>App</h1>
    <Row
      v-for="record in records"
      v-model="converted[record[keyField]]"
      v-bind="{ migrateFunction, schema, record }"
      :key="record[keyField]"
      :active="false"
      :completed="completed[record[keyField]]"
    >
      <template v-slot:input="{ record }"
        ><slot name="input" v-bind="{ record }"
      /></template>
    </Row>
  </div>
</template>

<script>
import Row from "@/components/Row.vue";
import { fromPairs } from "lodash-es";

export default {
  components: {
    Row,
  },
  data() {
    return {
      converted: fromPairs(this.records.map((r) => [r[this.keyField], {}])),
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
