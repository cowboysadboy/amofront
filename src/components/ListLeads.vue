<template>
  <section>
    <a-table :columns="columns" :data-source="title" class="components-table-demo-nested">
      <template #bodyCell="{ column, text }">
        <template v-if="column.dataIndex === 'status'">
          <span :style="{ background: text.color }">{{ text.name }}</span>
        </template>
        <template v-if="column.dataIndex === 'user'">
          <span>{{ text.name }}</span>
        </template>
        <template v-if="column.dataIndex === 'created_by'">
          <span>{{ formatDate(text) }}</span>
        </template>
      </template>
      <template #expandedRowRender="data">
        <div v-for="item in  data.record.contact_person " :key="item.id">
          {{ item.name }}
          <div v-for="elem in  item.contacts " :key="elem.field_id">
            {{ elem.field_name }}:
            <div v-if="elem.field_code === 'PHONE'">
              <ul v-for="value in  elem.values " :key="value.enum_id">
                <li>
                  <a :href="'tel:' + value.value">{{ value.value }}</a>
                </li>
              </ul>
            </div>

            <div v-if="elem.field_code === 'EMAIL'">
              <ul v-for="value in  elem.values " :key="value.enum_id">
                <li>
                  <a :href="'mailto:' + value.value">{{ value.value }}</a>
                </li>
              </ul>
            </div>
            <div v-if="elem.field_code === 'POSITION'">
              <ul v-for="value in  elem.values " :key="value.enum_id">
                <li>
                  {{ value.value }}
                </li>
              </ul>
            </div>

          </div>
        </div>
      </template>
    </a-table>
  </section>
</template>
<script lang="ts" setup>
import { defineProps } from 'vue';
import { Leads } from '../types/types'

defineProps<{
  title?: Leads[]
}>()

const columns = [
  { title: 'Название', dataIndex: 'name', key: 'name' },
  { title: 'Бюджет', dataIndex: 'price', key: 'price' },
  { title: 'Статус', dataIndex: 'status', key: 'status' },
  { title: 'Ответственный', dataIndex: 'user', key: 'user' },
  { title: 'Дата создания', dataIndex: 'created_by', key: 'created_by' },
];

const formatDate = (timestamp: number): string => {
  const date = new Date(timestamp * 1000);
  const day = date.getDate().toString().padStart(2, '0');
  const month = (date.getMonth() + 1).toString().padStart(2, '0');
  const year = date.getFullYear();

  return `${day}.${month}.${year}`;
}
</script>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ListLeads',
});
</script>

