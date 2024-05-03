<template>
  <div>
    <div v-for="item in verticalList" :key="item.id" class="vertical-item">
      <div class="horizontal-list">
        <div v-for="number in item.numbers" :key="number.id" class="square" @mouseenter="startHover(number)" @mouseleave="stopHover(number)">
          {{ number.value }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const verticalList = ref([]);
const updateInterval = 5000; // 5сек.

const generateRandomNumber = () => Math.floor(Math.random() * 100);

const generateRandomList = (length) => {
  const list = [];
  for (let i = 0; i < length; i++) {
    list.push({ id: i, value: generateRandomNumber() });
  }
  return list;
};

const generateVerticalList = (length) => {
  const list = [];
  for (let i = 0; i < length; i++) {
    list.push({ id: i, numbers: generateRandomList(Math.floor(Math.random() * 10) + 10) });
  }
  return list;
};

const startHover = (number) => {
  number.hovered = true;
};

const stopHover = (number) => {
  number.hovered = false;
};

const updateNumbers = () => {
  verticalList.value.forEach((item) => {
    item.numbers.forEach((number) => {
      if (!number.hovered) {
        number.value = generateRandomNumber();
      }
    });
  });
};

onMounted(() => {
  verticalList.value = generateVerticalList(Math.floor(Math.random() * 100) + 100);

  setInterval(updateNumbers, updateInterval);
});

onUnmounted(() => {
  clearInterval(updateNumbers);
});
</script>

<style scoped>
.square {
  width: 50px;
  height: 50px;
  border: 1px solid black;
  border-radius: 10px;
  margin: 5px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
}

.square:hover {
  transform: scale(0.8);
}

.vertical-item {
  margin-bottom: 20px;
}

.horizontal-list {
  display: flex;
  flex-wrap: wrap;
}
</style>
