<template>
  <div style="padding: 30px">
    <a-card title="Список заявок" :bordered="false" style="width: 100%">
      <a-input-search v-model:value="value" placeholder="input search text"
        style="max-width:300px; margin: 10px auto 20px;" @search="getData" />
      <ListLeads :title="items" />
    </a-card>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import ListLeads from './components/ListLeads.vue';

export default defineComponent({
  name: 'App',
  components: {
    ListLeads
  },
  setup() {
    const items = ref([]);
    const value = ref<string>('');
    const onSearch = (searchValue: string) => {
      console.log('use value', searchValue);
      console.log('or use this.value', value.value);
    };
    function getData() {
      fetch(`http://localhost:3000/api/leads?query=${value.value}`)
        .then(response => response.json())
        .then(data => {
          console.log(data);
          items.value = data;
        })
        .catch(error => {
          console.error(error);
        });
    }
    getData();
    return {
      items,
      onSearch,
      value,
      getData
    };
  }
});
</script>