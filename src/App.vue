<script setup lang="ts">
import { ref } from 'vue';
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-quartz.css";
import { AgGridVue } from "ag-grid-vue3";
import CustomImage from './components/CustomImage.vue';

type Emojis = {
  name: string
  id?: string
  category: string
  aliases: string[]
  url: string
}
type EmojisResponce = {
  emojis: Emojis[]
}

const colDefs = ref([
  { field: "name", filter: true },
  { field: "category", filter: true },
  { field: "aliases", filter: true },
  { field: "url", filter: true, cellRenderer: CustomImage }
])

const rowData = ref<Emojis[]>([])

const serverUrl = ref("");
const getEmojis = async () => {
  serverUrl.value.replace("https://", "");
  serverUrl.value.replace("http://", "");
  const res = await fetch(`https://${serverUrl.value}/api/emojis`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({}),
  });
  const emojis = await res.json() as EmojisResponce
  rowData.value = emojis.emojis
  console.log(emojis.emojis)
}

</script>

<template>
  <input type="url" v-model="serverUrl" placeholder="サーバーURL(ex:voskey.icalo.net)">
  <button @click="getEmojis">取得</button>
  <AgGridVue :rowData="rowData" :columnDefs="colDefs" rowHeight="50px" class="ag-theme-quartz" style="height: 95svh;"></AgGridVue>
</template>