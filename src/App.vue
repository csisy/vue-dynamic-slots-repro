<script setup lang="ts">
import MyTable, { type ColumnDef } from './MyTable.vue';

type RowData = {
  id: number;
  foo: number;
  bar: string;
};

const columns: ColumnDef<RowData>[] = [
  {
    property: 'foo',
    title: 'First column',
  },
  {
    property: 'bar',
    title: 'Second column',
  },
];
const data: RowData[] = [
  { id: 1, foo: 42, bar: 'hello' },
  { id: 2, foo: 24, bar: 'world' },
];
</script>

<template>
  <MyTable :columns="columns" :data="data">
    <!-- we can define column-id, column-foo and column-bar -->
    <!-- and the type of value is inferred properly -->
    <template #column-foo="{ value }">
      <b>{{ value }}</b>
    </template>

    <!-- however, we cannot define anything else since it "does not exist on type" -->
    <!-- <template #column-baz="{ value }">
      <b>{{ value }}</b>
    </template> -->
  </MyTable>
</template>
