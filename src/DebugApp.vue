<template>
  <div>
    <BatchApprover
      v-bind="{ records, schema, migrateFunction, confirmFunction }"
    >
      <template v-slot:input="{ record }">
        <h3>{{ record.name }}</h3>
      </template>
    </BatchApprover>
  </div>
</template>

<script>
import BatchApprover from "@/components/BatchApprover";

const records = [
  { name: "Hamburger", id: 0 },
  { name: "Pizza", id: 1 },
];

const CUISINES = ["American", "Italian"];
const schema = [
  {
    type: "text",
    name: "name",
    label: "Name of dish",
    validation: "required",
  },
  {
    type: "radio",
    name: "cuisine",
    label: "Cuisine",
    options: CUISINES.map((c) => ({ value: c, id: c, label: c })),
    validation: "required",
  },
];

export default {
  components: { BatchApprover },
  data() {
    return { records, schema };
  },
  methods: {
    async migrateFunction(record) {
      return fetch("https://reqres.in/api/products/3").then(function () {
        return {
          ...record,
          cuisine: CUISINES[record.id],
        };
      });
    },
    confirmFunction(record) {
      console.log("confirmed");
      return record;
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
