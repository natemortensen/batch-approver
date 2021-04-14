<template>
  <div>
    <BatchApprover v-bind="{ records, schema, migrateFunction }">
      <template v-slot:input="{ record }">
        <h3>{{ record.name }}</h3>
      </template>
    </BatchApprover>
  </div>
</template>

<script>
import BatchApprover from "@/components/BatchApprover";

const records = [
  { name: "Hamburger", id: 1 },
  { name: "Pizza", id: 2 },
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
  {
    name: "submit",
    type: "submit",
    label: "Submit Changes",
  },
];

export default {
  components: { BatchApprover },
  data() {
    return { records, schema };
  },
  methods: {
    migrateFunction(record) {
      return {
        ...record,
        cuisine: CUISINES[record.id],
      };
    },
  },
};
</script>
