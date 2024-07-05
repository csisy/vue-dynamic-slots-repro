<script setup lang="ts" generic="T extends { [K in keyof T]: T[K] } & { id: PropertyKey }">
import { ref } from 'vue';

// definition of a column
export type ColumnDef<T> = {
  // the property is a key of the generic type
  property: keyof T & string;
  title: string;
};

// the columns and data rows must be specified as inputs
type Props = {
  columns: ColumnDef<T>[];
  data: T[];
};

// this is the type of the column template; receives the given row and the value based on the given 'property' of the column def.
type ColumnTemplateData<K extends keyof T> = {
  row: T;
  value: T[K];
};

const props = defineProps<Props>();

// we will have slots in the `column-{keyof T}` format with these props
defineSlots<{
  [K in keyof T as `column-${K & string}`]: (_: ColumnTemplateData<K>) => any;
}>();

const columns = ref(
  props.columns.map((definition) => ({
    ...definition,
    width: 150,
  })),
);
</script>

<template>
  <div class="table">
    <div v-for="row in data" :key="row.id" class="row">
      <div
        v-for="col in columns"
        :key="col.property"
        class="col"
        :style="{ width: `${col.width}px` }"
      >
        <!-- error: Type '`column-${UnwrapRef<keyof T & string>}`' cannot be used to index type [...] -->
        <slot :name="`column-${col.property}`" :row="row" :value="row[col.property]">
          <span>{{ row[col.property] }}</span>
        </slot>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.table {
  display: flex;
  flex-direction: column;
  width: fit-content;
}

.row {
  display: flex;
}
</style>
