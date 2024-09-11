<template>
  <div class="table-wrapper">
    <table>
      <tr v-for="(period, indexP) in periods" :key="indexP">
        <td v-for="(group, indexG) in groups" :key="indexG">
          <div
            :class="{
              gas: findElement(group, period).phase === 'Gas',
              solid: findElement(group, period).phase !== 'Gas',
            }"
            class="element"
            v-if="findElement(group, period)"
            @click="openDialog(findElement(group, period))"
          >
            <div class="electro">
              {{ findElement(group, period).electronegativity_pauling }}
            </div>
            <div class="symbol">
              <sub>{{ findElement(group, period).number }}</sub
              >{{ findElement(group, period).symbol }}
            </div>
            <div class="name">{{ findElement(group, period).name }}</div>
            <div class="mass">{{ findElement(group, period).atomic_mass }}</div>
          </div>
        </td>
      </tr>
    </table>
  </div>
  <DialogWindow
    @close="closeDialog"
    v-if="showDialog"
    :element="dataOfElement"
  />
</template>
<script setup>
import DialogWindow from "./DialogWindow.vue";
import { onMounted, ref } from "vue";
let groups = ref([]),
  periods = ref([]);
const showDialog = ref(false),
  dataOfElement = ref({});
for (let i = 1; i <= 18; i++) {
  groups.value.push(i);
}
for (let i = 1; i <= 7; i++) {
  periods.value.push(i);
}
const jsonData = ref([]);

const fetchPeriodTable = async () => {
  const response = await fetch(
    "https://raw.githubusercontent.com/Bowserinator/Periodic-Table-JSON/master/PeriodicTableJSON.json"
  );
  const data = await response.json();
  jsonData.value = data.elements;
};

const findElement = (group, period) => {
  return jsonData.value.find(
    (element) => element.group == group && element.period == period
  );
};
const openDialog = (item) => {
  showDialog.value = true;
  dataOfElement.value = item;
};
const closeDialog = () => {
  showDialog.value = false;
};
onMounted(fetchPeriodTable);
</script>
<style>
.table-wrapper {
  display: flex;
  justify-content: center;
}
table {
  margin-top: 10px;
  width: 90%;
}
td {
  text-align: center;
  flex-direction: column;
  border-collapse: collapse;
}
sub {
  font-size: 13px;
}
.symbol {
  font-size: 40px;
}
.name {
  font-size: 18px;
}
.element {
  border: 1px solid white;
  position: relative;
  transition: transform 0.3s ease;
}
.electro {
  position: absolute;
  color: rgb(255, 60, 60);
  font-size: 12px;
  left: 5%;
}
.mass {
  font-size: 11px;
}
.gas {
  background-color: rgba(255, 166, 1, 0.616);
}
.solid {
  background-color: rgb(0, 117, 117);
}
.element:hover {
  transform: scale(1.2);
  z-index: 1000;
  cursor: pointer;
}
</style>
