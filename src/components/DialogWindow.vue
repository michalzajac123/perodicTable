<template>
  <div class="backdrop">
    <div class="content">
      <div class="electro">{{ props.element.electronegativity_pauling }}</div>
      <div class="symbol">
        <sub>{{ props.element.number }}</sub
        >{{ props.element.symbol }}
      </div>
      <div class="name">{{ props.element.name }}</div>
      <div class="mass">{{ props.element.atomic_mass }} u</div>
      <button @click="close()">Zamknij</button>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps(["element"]);
const emit = defineEmits(["close"]);
function close() {
  emit("close");
}
</script>

<style scoped>
.content {
  text-align: center;
  flex-direction: column;
  display: flex;
  justify-content: center;
  align-items: center;
}
.backdrop {
  bottom: 50%;
  right: 40%;
  position: fixed;
  background-color: rgb(50, 50, 50);
  width: 450px;
  height: 450px;
  z-index: 1100;
}
.symbol {
  font-size: 150px;
}
.name {
  font-size: 40px;
}
sub {
  font-size: 18px;
}
.element {
  border: 1px solid white;
  position: relative;
  transition: transform 0.3s ease;
}
.electro {
  color: rgb(255, 60, 60);
  font-size: 60px;
  top: 2px;
}
.mass {
  font-size: 20px;
}
.gas {
  background-color: rgba(255, 166, 1, 0.616);
}
.solid {
  background-color: rgb(0, 117, 117);
}
button {
  margin: 10px;
  width: 20%;
  font-size: 20px;
  border: 0;
  background-color: rgb(50, 50, 50);
  border-radius: 15px;
  background-color: rgba(255, 166, 1, 0.616);
  transition: 0.2s;
}
button:hover {
  background-color: rgb(0, 117, 117);
  color: white;
  cursor: pointer;
}
</style>
